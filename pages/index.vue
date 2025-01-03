<script setup lang="ts">
useSeoMeta({
  title: "PokeNuxt",
  description: "Poke Api X Nuxt.js",
});

const limit = 18;
const route = useRoute();
const router = useRouter();
const offset = ref(Number(route.query.offset) || 0);
const searchQuery = ref("");
const searchedData = ref<PokemonDetail[] | null>(null);
const error = ref("");

interface Pokemon {
  name: string;
  image: string;
}

interface PokemonApiResponse {
  results: Pokemon[];
}

interface PokemonDetail {
  name: string;
  image: string;
}

const fetchPokemon = async (offset: number) => {
  const response = await $fetch<PokemonApiResponse>(
    `https://pokeapi.co/api/v2/pokemon?limit=${limit}&offset=${offset}`
  );

  const pokemonLists = response.results;

  const pokemonDetails: PokemonDetail[] = await Promise.all(
    pokemonLists.map(async (pokemon) => {
      const res = await fetch(
        `https://pokeapi.co/api/v2/pokemon/${pokemon.name}`
      );
      const pokemonData = await res.json();
      return {
        name: pokemonData.name,
        image: pokemonData.sprites.front_default,
      };
    })
  );

  return pokemonDetails;
};

const { data, refresh, pending } = useAsyncData("fetchPokemon", () =>
  fetchPokemon(offset.value)
);

const searchPokemon = async () => {
  if (!searchQuery.value.trim()) {
    searchedData.value = null;
    error.value = "";
    return refresh();
  }

  try {
    const response = await fetch(
      `https://pokeapi.co/api/v2/pokemon/${searchQuery.value.toLowerCase()}`
    );
    const pokemonData = await response.json();
    searchedData.value = [
      {
        name: pokemonData.name,
        image: pokemonData.sprites.front_default,
      },
    ];
    error.value = "";
  } catch (err) {
    console.error("Pokemon not found", err);
    searchedData.value = [];
    error.value = `There is no Pokémon with the name "${searchQuery.value}"`;
  }
};

const updatePaginationInURL = () => {
  router.push({
    query: {
      ...route.query,
      offset: offset.value.toString(),
    },
  });
};

const nextPage = () => {
  offset.value += limit;
  updatePaginationInURL();
  refresh();
  scrollTop();
};

const prevPage = () => {
  if (offset.value > 0) {
    offset.value -= limit;
    updatePaginationInURL();
    refresh();
    scrollTop();
  }
};

const scrollTop = () => {
  window.scrollTo({
    top: 0,
    behavior: "smooth",
  });
};

const handleBack = () => {
  searchedData.value = null;
  error.value = "";
  refresh();
};
</script>

<template>
  <div>
    <div class="flex gap-x-2 items-center mb-7">
      <input
        type="text"
        placeholder="🔎 Search Pokemon.."
        class="border py-2 px-4 rounded-lg outline-blue-500 w-full max-w-sm"
        v-model="searchQuery"
        @keyup.enter="searchPokemon"
      />
      <UiButton :text="`Search`" :onClick="searchPokemon" />
    </div>
    <div class="grid grid-cols-2 sm:grid-cols-3 gap-2 sm:gap-4">
      <UiCard
        v-for="pokemon in searchedData || data"
        :key="pokemon.name"
        :name="pokemon.name"
        :image="pokemon.image"
      />
    </div>
    <div v-if="error" class="text-red-500 mb-4">{{ error }}</div>
    <div v-if="error">
      <UiButton :text="`Back`" :onClick="handleBack" />
    </div>

    <div v-if="!searchedData" class="flex gap-x-2 mt-7 justify-end">
      <UiButton :text="`Prev`" :onClick="prevPage" :disabled="offset === 0" />
      <UiButton :text="`Next`" :onClick="nextPage" :disabled="pending" />
    </div>
  </div>
</template>

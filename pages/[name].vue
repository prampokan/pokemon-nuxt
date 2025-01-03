<script lang="ts" setup>
import { ref } from "vue";
import { useRouter, useRoute } from "vue-router";

const router = useRouter();
const route = useRoute();
const hoveredImage = ref("");

const { data } = await useAsyncData<any>("fetchEachPokemon", () =>
  $fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.name}`)
);

const onHover = (sprite: string) => {
  hoveredImage.value = sprite;
};

useSeoMeta({
  title: `${
    Array.isArray(route.params.name)
      ? route.params.name[0]
      : route.params.name?.charAt(0).toUpperCase() + route.params.name?.slice(1)
  } - PokeNuxt`,
  description: "Poke Api X Nuxt.js",
});
</script>

<template>
  <UiButton :text="`Back`" @click="router.back" />
  <div class="mt-7">
    <div class="grid grid-cols-1 sm:grid-cols-2 gap-y-10">
      <div>
        <h1 class="capitalize font-bold text-2xl underline">
          {{ data.name }}
        </h1>
        <div class="lg:w-[25rem] lg:h-[25rem] mt-7">
          <img
            :src="hoveredImage || data.sprites.front_default"
            alt="pokemon"
            class="w-full h-full"
          />
        </div>
        <div class="grid grid-cols-4 lg:w-[25rem]">
          <div
            class="lg:w-24 lg:h-24 hover:bg-zinc-100 transition-all rounded-lg cursor-pointer"
            @mouseover="onHover(data.sprites.front_default)"
          >
            <img :src="data.sprites.front_default" :alt="data.name" />
          </div>
          <div
            class="lg:w-24 lg:h-24 hover:bg-zinc-100 transition-all rounded-lg cursor-pointer"
            @mouseover="onHover(data.sprites.back_default)"
          >
            <img :src="data.sprites.back_default" :alt="data.name" />
          </div>
          <div
            class="lg:w-24 lg:h-24 hover:bg-zinc-100 transition-all rounded-lg cursor-pointer"
            @mouseover="onHover(data.sprites.front_shiny)"
          >
            <img :src="data.sprites.front_shiny" :alt="data.name" />
          </div>
          <div
            class="lg:w-24 lg:h-24 hover:bg-zinc-100 transition-all rounded-lg cursor-pointer"
            @mouseover="onHover(data.sprites.back_shiny)"
          >
            <img :src="data.sprites.back_shiny" :alt="data.name" />
          </div>
        </div>
      </div>
      <div>
        <div class="flex gap-x-2">
          <UiBadge
            v-for="type in data.types"
            :key="type.type.name"
            :text="type.type.name"
          />
        </div>
        <div class="mt-5 flex gap-x-4">
          <UiMiniCard
            :value="data.weight / 10"
            :text="`Weight`"
            :icon="'Weight'"
            :unit="`kg`"
          />
          <UiMiniCard
            :value="data.height / 10"
            :text="`Height`"
            :icon="'Ruler'"
            :unit="`m`"
          />
        </div>
        <div class="mt-5 flex flex-col md:flex-row md:items-center gap-2">
          <p class="text-zinc-600">Ability :</p>
          <UiBadge
            v-for="ability in data.abilities"
            :key="ability.ability.name"
            :text="ability.ability.name"
          />
        </div>
        <div class="mt-5 flex flex-col gap-y-4">
          <UiStatusBar
            v-for="stat in data.stats"
            :key="stat.stat.name"
            :name="stat.stat.name"
            :stat="stat.base_stat"
          />
        </div>
      </div>
    </div>
  </div>
</template>

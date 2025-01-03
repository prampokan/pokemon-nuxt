<script setup>
const { name = "...", image } = defineProps({
  name: String,
  image: String,
});
</script>

<template>
  <RouterLink
    :to="`/${name}`"
    class="rounded-md border w-auto overflow-hidden cursor-pointer hover:shadow-xl hover:scale-105 transition-all group"
  >
    <div class="flex justify-center items-center w-full h-60 py-2">
      <img :src="image" alt="image" class="object-contain w-full h-full" />
    </div>
    <div class="p-4 border-t text-center">
      <h1
        class="font-semibold group-hover:text-sky-500 text-zinc-700 capitalize"
      >
        {{ name }}
      </h1>
    </div>
  </RouterLink>
</template>

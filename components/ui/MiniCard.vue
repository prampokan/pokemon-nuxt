<script setup>
import * as lucide from "lucide-vue-next";
const {
  value = "...",
  text = "...",
  unit,
  icon,
} = defineProps({
  value: Number,
  text: String,
  unit: String,
  icon: String,
});
</script>

<template>
  <div
    class="w-32 h-32 bg-zinc-100 rounded-lg shadow flex justify-center items-center relative"
  >
    <div class="p-2 bg-zinc-200 rounded-lg absolute top-2 right-2 shadow-sm">
      <component :is="lucide[icon]" :size="16" />
    </div>
    <h1 class="font-bold text-2xl">{{ value }} {{ unit }}</h1>
    <p class="absolute bottom-4 text-sm text-zinc-500">{{ text }}</p>
  </div>
</template>

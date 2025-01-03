<script lang="ts" setup>
const { text = "..." } = defineProps({
  text: String,
});
</script>

<template>
  <div class="px-4 py-2 bg-zinc-100 font-bold rounded-lg shadow uppercase">
    {{ text }}
  </div>
</template>

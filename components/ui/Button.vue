<script lang="ts" setup>
const { text = "...", onClick = () => {} } = defineProps({
  text: String,
  onClick: {
    type: Function as PropType<(event: MouseEvent) => void>,
    default: () => {},
  },
});
</script>

<template>
  <button
    class="px-4 py-2 border rounded-lg font-semibold hover:bg-zinc-100 transition-all disabled:bg-zinc-100"
    @click="onClick"
  >
    {{ text }}
  </button>
</template>

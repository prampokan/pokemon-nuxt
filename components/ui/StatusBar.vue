<script setup>
import { Sword } from "lucide-vue-next";
import { Swords } from "lucide-vue-next";
import { Heart } from "lucide-vue-next";
import { Shield } from "lucide-vue-next";
import { ShieldPlus } from "lucide-vue-next";
import { Zap } from "lucide-vue-next";
import { computed } from "vue";

const { name = "...", stat } = defineProps({
  name: String,
  stat: Number,
});

const percentage = computed(() => {
  return (stat / 200) * 100;
});
</script>

<template>
  <div
    class="w-full h-14 bg-zinc-100 rounded-lg shadow px-2 flex items-center gap-x-2"
  >
    <div class="w-10 h-10 flex justify-center items-center text-zinc-700">
      <Sword v-if="name === 'attack'" />
      <Heart v-else-if="name === 'hp'" />
      <Shield v-else-if="name === 'defense'" />
      <Swords v-else-if="name === 'special-attack'" />
      <ShieldPlus v-else-if="name === 'special-defense'" />
      <Zap v-else-if="name === 'speed'" />
    </div>
    <div class="w-full">
      <p class="text-sm mb-1 font-bold text-zinc-900 uppercase">
        {{ name }} : {{ stat }}
      </p>
      <div class="w-full h-2 bg-zinc-200 rounded-full">
        <div
          :class="{
            'h-2 rounded-full transition-all duration-300': true,
            'bg-red-500': name === 'attack',
            'bg-blue-500': name === 'hp',
            'bg-zinc-500': name === 'defense',
            'bg-orange-500': name === 'special-attack',
            'bg-slate-500': name === 'special-defense',
            'bg-yellow-500': name === 'speed',
          }"
          :style="{ width: `${percentage}%` }"
        ></div>
      </div>
    </div>
  </div>
</template>

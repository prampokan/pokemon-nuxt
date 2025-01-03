<script lang="ts" setup>
import { Github } from "lucide-vue-next";
</script>

<template>
  <nav
    class="w-full border-b flex items-center justify-center h-14 px-5 fixed font-mono bg-white/50 backdrop-blur z-50"
  >
    <div class="max-w-6xl w-full flex justify-between items-center">
      <a href="/" class="flex gap-x-2 cursor-pointer">
        <img src="/images/nuxt.ico" alt="" />
        <img src="/images/pokeapi.png" alt="" class="w-20" />
      </a>
      <a
        href="https://github.com/prampokan/pokemon-nuxt"
        target="_blank"
        class="flex items-center gap-x-1 cursor-pointer font-bold hover:text-green-500 transition-all"
      >
        <Github :size="16" />
        Github
      </a>
      <!-- other nav -->
    </div>
  </nav>
</template>

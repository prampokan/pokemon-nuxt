<template>
  <AppNavbar />
  <main
    class="w-full flex justify-center pt-20 pb-10 font-mono px-5 min-h-screen"
  >
    <div class="max-w-5xl w-full">
      <slot />
    </div>
  </main>
  <AppFooter />
</template>

<script setup>
defineProps(['playerName'])
defineEmits(['restart'])
</script>

<template>
  <section class="flex flex-col items-center justify-center flex-1 gap-8 p-6 text-center animate-fade-in">
    <div class="text-8xl md:text-9xl animate-bounce drop-shadow-[0_0_30px_rgba(255,215,0,0.5)]">🏆</div>
    
    <div class="space-y-2">
      <h2 class="text-5xl md:text-7xl text-yellow-400 font-black tracking-tighter italic">MISSION COMPLETE</h2>
      <div class="h-1 w-24 bg-cyan-500 mx-auto rounded-full"></div>
    </div>

    <p class="text-2xl md:text-3xl text-cyan-300 font-bold">
      ยอดเยี่ยม! <span class="text-white">{{ playerName }}</span> ชนะบอสทั้งหมดแล้ว
    </p>

    <button
      @click="$emit('restart')"
      class="flex justify-center items-center gap-3 relative px-12 py-4 rounded-2xl font-black text-xl tracking-widest text-yellow-200 border-2 border-yellow-500 bg-gradient-to-r from-yellow-700 via-yellow-600 to-yellow-700 shadow-[0_0_20px_rgba(255,215,0,0.4)] transition-all duration-200 hover:-translate-y-1 hover:scale-110 hover:shadow-[0_0_30px_rgba(255,215,0,0.8)] hover:brightness-125 active:scale-95 active:translate-y-0 cursor-pointer"
    >
      <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2.5" stroke="currentColor" class="size-7">
        <path stroke-linecap="round" stroke-linejoin="round" d="M16.023 9.348h4.992v-.001M2.985 19.644v-4.992m0 0h4.992m-4.993 0 3.181 3.183a8.25 8.25 0 0 0 13.803-3.7M4.031 9.865a8.25 8.25 0 0 1 13.803-3.7l3.181 3.182m0-4.991v4.99" />
      </svg>
      PLAY AGAIN
    </button>
  </section>
</template>

<style scoped>
</style>
<script setup>
defineProps(['playerName'])
defineEmits(['restart'])
</script>
<template>
  <section class="flex flex-col items-center justify-center flex-1 gap-6">
    <div class="text-6xl ">🏆</div>
    <h2 class="text-5xl text-yellow-400 font-extrabold">MISSION COMPLETE</h2>
    <p class="text-center text-xl text-cyan-300">
      {{ playerName }} ชนะทั้งหมด
    </p>
    <button
      @click="$emit('restart')"
      class="flex justify-center items-center gap-1 relative px-8 py-3 rounded-lg font-bold tracking-widest text-yellow-200 border border-yellow-500 bg-gradient-to-r from-yellow-700 via-yellow-600 to-yellow-700 shadow-[0_0_15px_rgba(255,215,0,0.4)] transition-all duration-200 hover:-translate-y-1 hover:scale-105 hover:shadow-[0_0_25px_rgba(255,215,0,0.8)] hover:brightness-110 active:scale-95 active:translate-y-0 animate-pulse-glow">
      <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6">
        <path stroke-linecap="round" stroke-linejoin="round" d="M5.25 5.653c0-.856.917-1.398 1.667-.986l11.54 6.347a1.125 1.125 0 0 1 0 1.972l-11.54 6.347a1.125 1.125 0 0 1-1.667-.986V5.653Z" />
      </svg>RESTART
    </button>
  </section>
</template>

<style scoped>
</style>
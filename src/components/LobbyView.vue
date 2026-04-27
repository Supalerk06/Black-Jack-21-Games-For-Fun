<script setup>
defineProps(['playerName', 'character', 'characters'])
defineEmits(['update:playerName', 'update:character', 'start'])
</script>

<template>
  <section class="flex flex-col items-center justify-center flex-1 gap-6 p-6">


  <h1 class="text-6xl md:text-7xl font-black tracking-widest italic cursor-default transition-all duration-300 hover:scale-105">
    
    <span class="text-yellow-300 [text-shadow:_0_0_15px_#facc15,_0_0_30px_#facc15,_0_0_45px_#facc1580]">
      BLACK JACK
    </span>
    
    <span class="text-cyan-300 [text-shadow:_0_0_15px_#22d3ee,_0_0_30px_#22d3ee,_0_0_45px_#22d3ee80] ml-2">
      21
    </span>
    
  </h1>


    <input
      :value="playerName"
      @input="$emit('update:playerName', $event.target.value)"
      placeholder="ENTER NAME"
      class="px-4 py-2 bg-black/60 border border-yellow-700 text-center rounded-xl"
    />

    <div class="flex gap-8 justify-center flex-wrap">
      <button
        v-for="c in characters"
        :key="c.name"
        @click="$emit('update:character', c.name)"
        class="group w-48 h-64 rounded-2xl overflow-hidden border-2 transition-all duration-300 bg-black/50 backdrop-blur-sm hover:-translate-y-2 hover:scale-105 hover:shadow-[0_0_15px_rgba(255,215,0,0.4)]"
        :class="character === c.name ? 'border-cyan-400 shadow-[0_0_25px_rgba(0,255,255,0.6)]' : 'border-yellow-700'"
      >
        <div class="relative w-full h-full overflow-hidden">
          <img
            :src="c.img"
            alt=""
            class="w-full h-full object-cover transition duration-500 group-hover:scale-110"
            :class="character === c.name ? 'scale-105' : ''"
          />
          <div class="absolute inset-0 bg-gradient-to-t from-black/80 via-black/20 to-transparent"></div>
          <div class="absolute bottom-3 left-0 right-0 text-center">
            <span class="text-lg font-bold tracking-wide" :class="character === c.name ? 'text-cyan-300' : 'text-yellow-200'">
              {{ c.name }}
            </span>
          </div>
        </div>
      </button>
    </div>

    <button
      @click="$emit('start')"
      class="flex justify-center items-center gap-1 relative px-8 py-3 rounded-lg font-bold tracking-widest text-yellow-200 border border-yellow-500 bg-gradient-to-r from-yellow-700 via-yellow-600 to-yellow-700 shadow-[0_0_15px_rgba(255,215,0,0.4)] transition-all duration-200 hover:-translate-y-1 hover:scale-105 hover:shadow-[0_0_25px_rgba(255,215,0,0.8)] hover:brightness-110 active:scale-95 active:translate-y-0 animate-pulse-glow"
    >
      <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6">
        <path stroke-linecap="round" stroke-linejoin="round" d="M5.25 5.653c0-.856.917-1.398 1.667-.986l11.54 6.347a1.125 1.125 0 0 1 0 1.972l-11.54 6.347a1.125 1.125 0 0 1-1.667-.986V5.653Z" />
      </svg>START
    </button>
  </section>
</template>

<style scoped>
</style>
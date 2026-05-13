<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
defineProps(['playerName', 'character', 'characters', 'showError'])
defineEmits(['update:playerName', 'update:character', 'start'])

const showGuide = ref(false)

const handleKeydown = (e) => {
  if (e.key === 'Escape') {
    showGuide.value = false
  }
}

onMounted(() => {
  window.addEventListener('keydown', handleKeydown)
})

onUnmounted(() => {
  window.removeEventListener('keydown', handleKeydown)
})
</script>

<template>
  <section class="flex flex-col items-center justify-center flex-1 gap-8 p-6 max-w-4xl mx-auto w-full">
    <h1 class="text-4xl md:text-8xl font-black tracking-widest italic cursor-default transition-all duration-300 hover:scale-105 text-center">
      <span class="text-yellow-300 [text-shadow:_0_0_15px_#facc15,_0_0_30px_#facc15,_0_0_45px_#facc1580]">
        BLACK JACK <span class="text-cyan-300 [text-shadow:_0_0_15px_#22d3ee,_0_0_30px_#22d3ee,_0_0_45px_#22d3ee80]">21</span>
      </span>
    </h1>

    <div class="flex flex-col items-center gap-3 w-full max-w-md">
      <label class="text-yellow-500 font-bold tracking-widest text-lg">กรอกชื่อของคุณ</label>
      <input
        :value="playerName"
        @input="$emit('update:playerName', $event.target.value)"
        placeholder="ENTER NAME"
        class="w-full px-6 py-4 bg-black/60 border-2 text-center rounded-2xl text-2xl font-bold tracking-widest transition-all duration-300 focus:outline-none focus:scale-105 cursor-text"
        :class="showError && !playerName.trim() ? 'border-red-500 shadow-[0_0_15px_rgba(239,68,68,0.5)]' : 'border-yellow-700 focus:border-cyan-400'"
      />
      <p v-if="showError && !playerName.trim()" class="text-red-400 text-sm animate-bounce">กรุณากรอกชื่อก่อนเริ่มเกม!</p>
    </div>

    <div class="flex flex-col items-center gap-4 w-full">
      <label class="text-yellow-500 font-bold tracking-widest text-lg">เลือกตัวละครของคุณ</label>
      <div class="flex gap-4 md:gap-8 justify-center flex-wrap">
        <button
          v-for="c in characters"
          :key="c.name"
          @click="$emit('update:character', c.name)"
          class="group w-40 h-56 md:w-48 md:h-64 rounded-2xl overflow-hidden border-2 transition-all duration-300 bg-black/50 backdrop-blur-sm hover:-translate-y-2 hover:scale-105 hover:shadow-[0_0_15px_rgba(255,215,0,0.4)] cursor-pointer"
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
      <p v-if="showError && !character" class="text-red-400 text-sm animate-bounce">กรุณาเลือกตัวละครก่อนเริ่มเกม!</p>
    </div>

    <div class="flex flex-col md:flex-row gap-4">
      <button
        @click="showGuide = true"
        class="flex justify-center items-center gap-2 relative px-8 py-4 rounded-2xl font-black text-xl tracking-widest text-cyan-300 border-2 border-cyan-700 bg-cyan-900/30 shadow-[0_0_15px_rgba(6,182,212,0.3)] transition-all duration-200 hover:-translate-y-1 hover:scale-105 hover:bg-cyan-700/40 cursor-pointer"
      >
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2.5" stroke="currentColor" class="size-7">
          <path stroke-linecap="round" stroke-linejoin="round" d="M12 6.042A8.967 8.967 0 0 0 6 3.75c-1.052 0-2.062.18-3 .512v14.25A8.987 8.987 0 0 1 6 18c2.305 0 4.408.867 6 2.292m0-14.25a8.966 8.966 0 0 1 6-2.292c1.052 0 2.062.18 3 .512v14.25A8.987 8.987 0 0 0 18 18a8.967 8.967 0 0 0-6 2.292m0-14.25v14.25" />
        </svg> GUIDE
      </button>

      <button
        @click="$emit('start')"
        class="flex justify-center items-center gap-2 relative px-10 py-4 rounded-2xl font-black text-xl tracking-widest text-yellow-200 border-2 border-yellow-500 bg-gradient-to-r from-yellow-700 via-yellow-600 to-yellow-700 shadow-[0_0_20px_rgba(255,215,0,0.4)] transition-all duration-200 hover:-translate-y-1 hover:scale-110 hover:shadow-[0_0_30px_rgba(255,215,0,0.8)] hover:brightness-125 active:scale-95 active:translate-y-0 cursor-pointer"
      >
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2.5" stroke="currentColor" class="size-7">
          <path stroke-linecap="round" stroke-linejoin="round" d="M5.25 5.653c0-.856.917-1.398 1.667-.986l11.54 6.347a1.125 1.125 0 0 1 0 1.972l-11.54 6.347a1.125 1.125 0 0 1-1.667-.986V5.653Z" />
        </svg>START GAME
      </button>
    </div>

    <Transition name="fade">
      <div v-if="showGuide" @click.self="showGuide = false" class="fixed inset-0 z-50 flex items-center justify-center p-6 bg-black/90 backdrop-blur-md">
        <div class="bg-gray-900 border-2 border-yellow-700 rounded-3xl p-8 max-w-md w-full shadow-[0_0_50px_rgba(0,0,0,0.5)] animate-result-pop">
          <div class="flex justify-between items-center mb-6">
            <h2 class="text-3xl font-black text-yellow-400 italic">GUIDE BOOK</h2>
            <button @click="showGuide = false" class="text-gray-500 hover:text-white transition-colors cursor-pointer">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="size-8">
                <path stroke-linecap="round" stroke-linejoin="round" d="M6 18 18 6M6 6l12 12" />
              </svg>
            </button>
          </div>
          
          <ul class="space-y-4 text-yellow-100/80">
            <li class="flex gap-3">
              <span class="text-cyan-400 font-black">01</span>
              <span>เป้าหมายคือทำคะแนนให้ใกล้เคียง <strong class="text-yellow-400">21</strong> มากที่สุดแต่ห้ามเกิน</span>
            </li>
            <li class="flex gap-3">
              <span class="text-cyan-400 font-black">02</span>
              <span>กด <strong class="text-red-400">HIT</strong> เพื่อจั่วไพ่เพิ่ม หากคะแนนเกิน 21 คุณจะแพ้ทันที</span>
            </li>
            <li class="flex gap-3">
              <span class="text-cyan-400 font-black">03</span>
              <span>กด <strong class="text-cyan-400">STAY</strong> เพื่อหยุดและเทียบแต้มกับศัตรู</span>
            </li>
            <li class="flex gap-3">
              <span class="text-cyan-400 font-black">04</span>
              <span>ชนะศัตรูเพื่อลด <strong class="text-red-500">HP</strong> ของพวกเขาและผ่านด่าน</span>
            </li>
          </ul>

          <button @click="showGuide = false" class="w-full mt-8 py-3 bg-yellow-600 text-black font-black rounded-xl hover:bg-yellow-500 transition-colors cursor-pointer">
            เข้าใจแล้ว!
          </button>
        </div>
      </div>
    </Transition>
  </section>
</template>

<style scoped>
.fade-enter-active, .fade-leave-active { transition: opacity 0.3s ease; }
.fade-enter-from, .fade-leave-to { opacity: 0; }
</style>
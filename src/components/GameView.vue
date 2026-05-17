<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
const props = defineProps([
  'STAGES', 'stageIdx', 'stage', 'bossHp', 'playerCards', 'playerScore', 
  'dealerCards', 'dealerScore', 'dealerRevealed', 'playerName', 
  'character', 'characters', 'message', 'phase', 'bossHit', 'showResult'
])
const emit = defineEmits(['hit', 'stay', 'home'])

const showGuide = ref(false)
const showExitConfirm = ref(false)

const handleKeydown = (e) => {
  if (e.key === 'Escape') {
    showGuide.value = false
    showExitConfirm.value = false
  }
}

onMounted(() => {
  window.addEventListener('keydown', handleKeydown)
})

onUnmounted(() => {
  window.removeEventListener('keydown', handleKeydown)
})

const playerImg = props.characters.find(c => c.name === props.character)?.img
</script>

<template>
  <section class="relative flex flex-col flex-1 overflow-hidden">
    <div class="flex justify-between items-center px-4 py-2 bg-black/80 border-b border-yellow-800 z-30">
      <div class="flex items-center gap-4">
        <button 
          @click="showExitConfirm = true"
          class="p-2 hover:bg-white/10 rounded-full transition-colors text-yellow-500 cursor-pointer"
          title="Back to Home"
        >
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="size-6">
            <path stroke-linecap="round" stroke-linejoin="round" d="m2.25 12 8.954-8.955c.44-.439 1.152-.439 1.591 0L21.75 12M4.5 9.75v10.125c0 .621.504 1.125 1.125 1.125H9.75v-4.875c0-.621.504-1.125 1.125-1.125h2.25c.621 0 1.125.504 1.125 1.125V21h4.125c.621 0 1.125-.504 1.125-1.125V9.75M8.25 21h8.25" />
          </svg>
        </button>
        <div class="flex flex-col gap-1">
          <span class="text-[10px] text-yellow-600 uppercase font-bold">Progress</span>
          <div class="flex gap-1">
            <div v-for="(s,i) in STAGES" :key="i"
              class="w-2.5 h-2.5 rounded-full"
              :class="i === stageIdx ? 'bg-cyan-400' : i < stageIdx ? 'bg-yellow-600' : 'bg-gray-700'"
            />
          </div>
        </div>
      </div>
      
      <div class="flex flex-col items-center">
        <span class="text-[10px] text-yellow-600 uppercase font-bold">{{ stage.title }}</span>
        <div class="text-sm text-yellow-400 tracking-widest font-bold">{{ stage.name }}</div>
      </div>

      <div class="flex flex-col items-end gap-1">
        <span class="text-[10px] text-red-600 uppercase font-bold">Boss HP</span>
        <div class="flex gap-1">
          <div v-for="n in stage.hp" :key="n"
            class="w-3 h-3 rounded-full"
            :class="n <= bossHp ? 'bg-red-500' : 'bg-gray-800 shadow-inner'"
          />
        </div>
      </div>
    </div>

    <div class="relative flex-1 flex flex-col justify-between p-4 md:p-8">
      <div class="flex justify-end items-start gap-4 md:block">
        <div class="md:absolute md:top-12 md:right-[200px] lg:right-[350px] xl:right-[420px] flex flex-col items-end gap-2 mt-4 z-20">
          <span class="text-[10px] text-yellow-600 uppercase font-bold mr-1">Boss Cards</span>
          <div class="flex -space-x-3">
            <div v-for="(card,i) in dealerCards" :key="i"
              class="w-14 h-20 md:w-16 md:h-22 flex items-center justify-center border-2 rounded-xl bg-white border-yellow-800 text-2xl font-bold text-gray-900 shadow-xl transition-all duration-300">
              <span v-if="!card.hidden">{{ card.val }}</span>
              <span v-else class="text-yellow-700">?</span>
            </div>
          </div>
          <div class="bg-black/70 px-4 py-1 rounded-full border border-yellow-700 flex gap-2 items-center">
            <span class="text-xs text-yellow-600 font-bold">SCORE:</span>
            <div class="text-xl font-black text-red-400 flex items-center gap-1">
              {{ dealerRevealed ? dealerScore : '?' }}
            </div>
          </div>
        </div>
        
        <div class="md:absolute md:top-4 md:-right-12 lg:-right-24 xl:-right-32 w-32 md:w-[280px] lg:w-[450px] xl:w-[550px] overflow-hidden z-10">
          <img :src="stage.img" class="w-full scale-x-[-1] drop-shadow-[0_0_20px_rgba(255,0,0,0.3)] transition-transform duration-500" :class="bossHit ? 'animate-boss-hit' : ''" />
        </div>
      </div>

      <div class="flex items-center justify-center z-30 my-4">
        <div class="bg-black/60 backdrop-blur-md px-6 py-3 rounded-2xl border border-yellow-700 text-yellow-300 italic font-medium text-center shadow-2xl max-w-xs md:max-w-md">
          {{ message }}
        </div>
      </div>

      <div class="flex justify-start items-end gap-4 md:block">
        <div class="md:absolute md:bottom-4 md:-left-12 lg:-left-24 xl:-left-32 w-32 md:w-[300px] lg:w-[500px] xl:w-[600px] overflow-hidden z-10">
           <img :src="playerImg" class="w-full drop-shadow-[0_0_20px_rgba(0,255,255,0.3)] transition-transform duration-500" />
        </div>

        <div class="md:absolute md:bottom-12 md:left-[200px] lg:left-[350px] xl:left-[420px] flex flex-col items-start gap-2 mb-4 z-20">
          <span class="text-[10px] text-cyan-600 uppercase font-bold ml-1">Your Cards</span>
          <div class="flex -space-x-3">
            <div v-for="(card,i) in playerCards" :key="i"
              class="w-14 h-20 md:w-18 md:h-24 flex items-center justify-center border-2 rounded-xl bg-white border-cyan-700 text-2xl md:text-3xl font-black text-gray-900 shadow-xl animate-card-draw">
              {{ card.val }}
            </div>
          </div>
          <div class="bg-black/70 px-4 py-1 rounded-full border border-cyan-700 flex gap-2 items-center">
            <div class="text-lg font-black text-yellow-400 truncate max-w-[100px]">{{ playerName || 'PLAYER' }}</div>
            <div class="h-4 w-[1px] bg-cyan-900"></div>
            <div class="text-cyan-400 text-xl font-black flex items-center">
              <span class="text-xs mr-1 opacity-70">SCORE:</span> {{ playerScore }}
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="bg-black/90 backdrop-blur-lg border-t border-yellow-800 p-4 md:p-6 z-30">
      <div class="flex flex-col gap-4 max-w-xl mx-auto">
        <div class="flex gap-4">
          <div class="flex-1 flex flex-col gap-1">
            <span class="text-[10px] text-red-500 font-bold uppercase text-center">เสี่ยงดวงเพิ่ม</span>
            <button
              @click="$emit('hit')"
              :disabled="phase !== 'PLAYER'"
              class="flex items-center justify-center gap-2 cursor-pointer font-black text-xl w-full py-4 border-2 border-red-700 bg-red-900/40 text-red-100 rounded-2xl hover:bg-red-700/60 transition-all active:scale-95 disabled:opacity-20 disabled:cursor-not-allowed shadow-[0_4px_0_rgb(127,29,29)] active:shadow-none active:translate-y-[4px]">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2.5" stroke="currentColor" class="size-6">
                <path stroke-linecap="round" stroke-linejoin="round" d="m3.75 13.5 10.5-11.25L12 10.5h8.25L9.75 21.75 12 13.5H3.75Z" />
              </svg> HIT
            </button>
          </div>
          
          <div class="flex-1 flex flex-col gap-1">
            <span class="text-[10px] text-cyan-500 font-bold uppercase text-center">พอแล้ว</span>
            <button
              @click="$emit('stay')"
              :disabled="phase !== 'PLAYER'"
              class="flex items-center justify-center gap-2 cursor-pointer font-black text-xl w-full py-4 border-2 border-cyan-700 bg-cyan-900/40 text-cyan-100 rounded-2xl hover:bg-cyan-700/60 transition-all active:scale-95 disabled:opacity-20 disabled:cursor-not-allowed shadow-[0_4px_0_rgb(8,145,178)] active:shadow-none active:translate-y-[4px]">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2.5" stroke="currentColor" class="size-6">
                <path stroke-linecap="round" stroke-linejoin="round" d="M9 12.75 11.25 15 15 9.75m-3-7.036A11.959 11.959 0 0 1 3.598 6 11.99 11.99 0 0 0 3 9.749c0 5.592 3.824 10.29 9 11.623 5.176-1.332 9-6.03 9-11.622 0-1.31-.21-2.571-.598-3.751h-.152c-3.196 0-6.1-1.248-8.25-3.285Z" />
              </svg> STAY
            </button>
          </div>
        </div>
        
        <div class="flex justify-between items-center text-xs tracking-tighter">
          <div class="text-yellow-600 font-bold">GOAL: 21</div>
          <button @click="showGuide = true" class="text-cyan-400 font-bold border-b border-cyan-900 hover:text-cyan-200 transition-colors cursor-pointer">วิธีเล่น (GUIDE)</button>
          <div class="text-yellow-600 font-bold">STATUS: {{ phase }}</div>
        </div>
      </div>
    </div>

    <Transition name="fade">
      <div v-if="showGuide" @click.self="showGuide = false" class="absolute inset-0 z-50 flex items-center justify-center p-6 bg-black/90 backdrop-blur-md">
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

    <Transition name="fade">
      <div v-if="showExitConfirm" @click.self="showExitConfirm = false" class="absolute inset-0 z-50 flex items-center justify-center p-6 bg-black/90 backdrop-blur-md">
        <div class="bg-gray-900 border-2 border-red-700 rounded-3xl p-8 max-w-md w-full shadow-[0_0_50px_rgba(239,68,68,0.3)] animate-result-pop">
          <div class="flex justify-center mb-6">
            <div class="w-20 h-20 bg-red-900/30 border-4 border-red-500 rounded-full flex items-center justify-center text-red-500 animate-pulse">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2.5" stroke="currentColor" class="size-10">
                <path stroke-linecap="round" stroke-linejoin="round" d="M12 9v3.75m-9.303 3.376c-.866 1.5.217 3.374 1.948 3.374h14.71c1.73 0 2.813-1.874 1.948-3.374L13.949 3.378c-.866-1.5-3.032-1.5-3.898 0L2.697 16.126ZM12 15.75h.007v.008H12v-.008Z" />
              </svg>
            </div>
          </div>
          
          <h2 class="text-3xl font-black text-white text-center mb-2 italic">ต้องการออกหรือไม่?</h2>
          <p class="text-gray-400 text-center mb-8 leading-relaxed">
            ความคืบหน้าปัจจุบันจะไม่ถูกบันทึก <br/>
            คุณจะต้องเริ่มเล่นใหม่ตั้งแต่ <strong class="text-yellow-500">ด่านแรก</strong>
          </p>

          <div class="flex gap-4">
            <button 
              @click="showExitConfirm = false" 
              class="flex-1 py-3 bg-gray-800 text-white font-bold rounded-xl hover:bg-gray-700 transition-colors cursor-pointer"
            >
              ยกเลิก
            </button>
            <button 
              @click="$emit('home')" 
              class="flex-1 py-3 bg-red-600 text-white font-black rounded-xl hover:bg-red-500 transition-colors shadow-[0_4px_0_rgb(153,27,27)] active:shadow-none active:translate-y-[4px] cursor-pointer"
            >
              ออกจากเกม
            </button>
          </div>
        </div>
      </div>
    </Transition>

    <div v-if="showResult" class="absolute inset-0 flex items-center justify-center z-40 bg-black/80 backdrop-blur-sm animate-fade-in">
      <div class="text-6xl md:text-8xl font-extrabold tracking-widest text-center animate-result-pop"
        :class="{
          'text-yellow-300 drop-shadow-[0_0_20px_rgba(255,215,0,0.8)]': showResult === 'WIN',
          'text-red-500 drop-shadow-[0_0_20px_rgba(255,0,0,0.7)]': showResult === 'LOSE',
          'text-gray-300': showResult === 'DRAW'
        }">
        {{ showResult }}
      </div>
    </div>
  </section>
</template>

<style scoped>
.fade-enter-active, .fade-leave-active { transition: opacity 0.3s ease; }
.fade-enter-from, .fade-leave-to { opacity: 0; }

@keyframes card-draw {
  from { opacity: 0; transform: translateY(20px) rotate(-5deg); }
  to { opacity: 1; transform: translateY(0) rotate(0); }
}
.animate-card-draw { animation: card-draw 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275) backwards; }

img {
  image-rendering: -webkit-optimize-contrast;
  backface-visibility: hidden;
  transform: translateZ(0);
}
</style>
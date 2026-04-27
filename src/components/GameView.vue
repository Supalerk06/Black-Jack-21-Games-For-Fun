<script setup>
defineProps([
  'STAGES', 'stageIdx', 'stage', 'bossHp', 'playerCards', 'playerScore', 
  'dealerCards', 'dealerScore', 'dealerRevealed', 'playerName', 
  'character', 'characters', 'message', 'phase', 'bossHit', 'showResult'
])
defineEmits(['hit', 'stay'])
</script>

<template>
  <section class="relative flex flex-col flex-1">
    <div class="flex justify-between px-4 py-2 bg-black/70 border-b border-yellow-800 z-20">
      <div class="flex gap-1">
        <div v-for="(s,i) in STAGES" :key="i"
          class="w-2.5 h-2.5 rounded-full"
          :class="i === stageIdx ? 'bg-cyan-400' : i < stageIdx ? 'bg-yellow-600' : 'bg-gray-700'"
        />
      </div>
      <div class="text-sm text-yellow-400 tracking-widest">{{ stage.title }}</div>
      <div class="flex gap-1">
        <div v-for="n in stage.hp" :key="n"
          class="w-3 h-3 rounded-full"
          :class="n <= bossHp ? 'bg-red-500' : 'bg-gray-800'"
        />
      </div>
    </div>

    <div class="relative flex-1">
      <div class="absolute bottom-0 left-0 flex items-end gap-6 p-6">
        <div class=" w-[800px] -mb-6 -ms-55 overflow-hidden">
           <img :src="characters.find(c => c.name === character)?.img" class="w-[900px]" />
        </div>
        <div class="flex flex-col items-start gap-2 mb-2 absolute left-110">
          <div class="flex -space-x-3">
            <div v-for="(card,i) in playerCards" :key="i"
              class="w-18 h-22 flex items-center justify-center border rounded bg-white border-yellow-700 text-xl text-yellow-700 shadow-lg">
              {{ card.val }}
            </div>
          </div>
          <div class="bg-black/70 px-4 py-1 rounded border border-yellow-700 flex gap-2 items-center">
            <div class="text-2xl font-extrabold text-yellow-400">{{ playerName || 'PLAYER' }}</div>
            <div class="text-cyan-400 text-2xl font-extrabold flex items-center">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="size-6">
                <path fill-rule="evenodd" d="M10.788 3.21c.448-1.077 1.976-1.077 2.424 0l2.082 5.006 5.404.434c1.164.093 1.636 1.545.749 2.305l-4.117 3.527 1.257 5.273c.271 1.136-.964 2.033-1.96 1.425L12 18.354 7.373 21.18c-.996.608-2.231-.29-1.96-1.425l1.257-5.273-4.117-3.527c-.887-.76-.415-2.212.749-2.305l5.404-.434 2.082-5.005Z" clip-rule="evenodd" />
              </svg>{{ playerScore }}
            </div>
          </div>
        </div>
      </div>

      <div class="absolute top-0 right-0 gap-20 flex items-start p-6">
        <div class="flex flex-col items-end gap-2 mt-6 -mr-20">
          <div class="flex -space-x-3">
            <div v-for="(card,i) in dealerCards" :key="i"
              class="w-16 h-20 flex items-center justify-center border rounded bg-white border-yellow-700 text-xl text-yellow-700 shadow-lg">
              <span v-if="!card.hidden">{{ card.val }}</span>
              <span v-else>?</span>
            </div>
          </div>
          <div class="bg-black/70 px-4 py-1 rounded border border-yellow-700 flex gap-2 items-center ">
            <div class="text-2xl font-extrabold text-yellow-400">{{ stage.name }}</div>
            <div class="text-red-400 text-2xl font-extrabold flex items-center gap-1">
              {{ dealerRevealed ? dealerScore : '?' }}
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="size-6">
                <path fill-rule="evenodd" d="M10.788 3.21c.448-1.077 1.976-1.077 2.424 0l2.082 5.006 5.404.434c1.164.093 1.636 1.545.749 2.305l-4.117 3.527 1.257 5.273c.271 1.136-.964 2.033-1.96 1.425L12 18.354 7.373 21.18c-.996.608-2.231-.29-1.96-1.425l1.257-5.273-4.117-3.527c-.887-.76-.415-2.212.749-2.305l5.404-.434 2.082-5.005Z" clip-rule="evenodd" />
              </svg>
            </div>
          </div>
        </div>
        <div class="w-[500px] overflow-hidden -mr-5">
          <img :src="stage.img" class="w-[500px] scale-x-[-1]" :class="bossHit ? 'animate-boss-hit' : ''" />
        </div>
      </div>

      <div class="absolute inset-0 flex items-center justify-center pointer-events-none">
        <div class="bg-black/60 px-6 py-3 rounded border border-yellow-700 text-yellow-300 italic">
          {{ message }}
        </div>
      </div>
    </div>

    <div class="bg-black/80 border-t border-yellow-800 p-4 z-20">
      <div class="flex gap-3 max-w-md mx-auto">
        <button
          @click="$emit('hit')"
          :disabled="phase !== 'PLAYER'"
          class="flex items-center justify-center gap-1 cursor-pointer font-bold text-xl flex-1 py-3 border border-red-700 bg-red-900/30 text-red-300 rounded-xl hover:bg-red-700/40 disabled:opacity-30">
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6">
            <path stroke-linecap="round" stroke-linejoin="round" d="m3.75 13.5 10.5-11.25L12 10.5h8.25L9.75 21.75 12 13.5H3.75Z" />
          </svg> HIT
        </button>
        <button
          @click="$emit('stay')"
          :disabled="phase !== 'PLAYER'"
          class="flex items-center justify-center gap-1 cursor-pointer font-bold text-xl flex-1 py-3 border border-cyan-700 bg-cyan-900/30 text-cyan-300 rounded-xl hover:bg-cyan-700/30 disabled:opacity-30">
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6">
            <path stroke-linecap="round" stroke-linejoin="round" d="M9 12.75 11.25 15 15 9.75m-3-7.036A11.959 11.959 0 0 1 3.598 6 11.99 11.99 0 0 0 3 9.749c0 5.592 3.824 10.29 9 11.623 5.176-1.332 9-6.03 9-11.622 0-1.31-.21-2.571-.598-3.751h-.152c-3.196 0-6.1-1.248-8.25-3.285Z" />
          </svg> STAY
        </button>
      </div>
      <div class="text-center mt-2 text-yellow-500">{{ playerScore }} / 21</div>
    </div>

    <div v-if="showResult" class="absolute inset-0 flex items-center justify-center z-30 bg-black/80 backdrop-blur-sm animate-fade-in">
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
<script setup>
import { ref } from 'vue'
import LobbyView from './components/LobbyView.vue'
import GameView from './components/GameView.vue'
import ResultView from './components/ResultView.vue'
import { computed } from 'vue'

const characters = [
  { name: 'มือปืนสี่ตา ', img: '/src/assets/player1real.gif' },
  { name: 'ค้อนยักบะลัก ', img: '/src/assets/player2.webp' },
  { name: 'มีอคู่ดู๋ดี๋ ', img: '/src/assets/player3.webp' }
]

const gameState = ref('LOBBY')


const playerName = ref('')
const character = ref('Aether-Knight')
const playerCards = ref([])
const dealerCards = ref([])
const phase = ref('PLAYER')
const message = ref('')

const rand = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min
function makeCard(hidden = false) { return { val: rand(1, 10), hidden } }
function calcScore(cards) { return cards.filter(c => !c.hidden).reduce((a, c) => a + c.val, 0) }

const playerScore = computed(() => calcScore(playerCards.value))
const dealerScore = computed(() => calcScore(dealerCards.value))


const STAGES = [
  { id: 1, name: 'Adt Master', title: 'Boss 1', img: '/src/assets/boss1.webp', stopAt: 15, hp: 3, bg: 'radial-gradient(ellipse at 50% 30%, #3d2800 0%, #1a0f00 60%, #0a0600 100%)', quote: '"เจ้ามีนามว่าอะไร !?"' },
  { id: 2, name: 'Art Aud Aud Art ', title: 'Boss 2', img: '/src/assets/boss2.webp', stopAt: 17, hp: 3, bg: 'radial-gradient(ellipse at 50% 30%, #3d0000 0%, #1a0000 60%, #050000 100%)', quote: '"อื้ด อืดอื้อ อ้าด อ้าด"' },
  { id: 3, name: 'Father Killer', title: 'Last Boss', img: '/src/assets/boss3.webp', stopAt: 18, hp: 3, bg: 'radial-gradient(ellipse at 50% 30%, #1a0030 0%, #0a0018 60%, #030008 100%)', quote: '"สวัดดี รู้มั้ยใครตาย...."' },
]

const stageIdx = ref(0)
const bossHp = ref(3)
const showResult = ref('') 
const dealerRevealed = ref(false)
const stage = computed(() => STAGES[stageIdx.value])
const delay = ms => new Promise(r => setTimeout(r, ms))

function startGame() {
  if (!playerName.value.trim()) return
  gameState.value = 'PLAYING'
  bossHp.value = stage.value.hp
  dealerRevealed.value = false
  startRound()
}

function startRound() {
  playerCards.value = [makeCard(), makeCard()]
  dealerCards.value = [makeCard(), makeCard(true)]
  phase.value = 'PLAYER'
  showResult.value = ''
  dealerRevealed.value = false
  message.value = stage.value.quote
}

function doHit() {
  if (phase.value !== 'PLAYER') return
  playerCards.value.push(makeCard())
  if (playerScore.value > 21) {
    message.value = '⚡ พลังงานล้นระบบ! Core Overload!'
    // รอเรียก trigger ใน commit ถัดไป
  } else {
    message.value = `เพิ่มพลังงาน → ${playerScore.value}`
  }
}

async function doStay() {
  if (phase.value !== 'PLAYER') return
  phase.value = 'DEALER'
  dealerCards.value = dealerCards.value.map(c => ({ ...c, hidden: false }))
  dealerRevealed.value = true
  message.value = `${stage.value.name} กำลังดำเนินการ...`
  await delay(800)
  while (calcScore(dealerCards.value) < stage.value.stopAt) {
    dealerCards.value.push(makeCard())
    message.value = `${stage.value.name} เพิ่มพลัง → ${calcScore(dealerCards.value)}`
    await delay(900)
  }
  const p = playerScore.value
  const d = dealerScore.value
  if (d > 21 || p > d) {
    message.value = '✦ ชัยชนะ! พลังงาน Aether ทำลายระบบ!'
  } else if (p < d) {
    message.value = '✸ ระบบล้มเหลว... ศัตรูมีพลังเหนือกว่า'
  } else {
    message.value = '◈ สมดุล! กำลังรีเซ็ตระบบ...'
    showResult.value = 'DRAW'
    await delay(2000)
    startRound()
  }
}

</script>

<template>
  <div class="min-h-screen flex flex-col text-yellow-100 bg-[radial-gradient(ellipse_at_50%_30%,#2a1800_0%,#0d0900_70%,#050300_100%)]">
    
    <LobbyView 
      v-if="gameState === 'LOBBY'"
      v-model:playerName="playerName"
      v-model:character="character"
      :characters="characters"
      @start="startGame"
    />

    <GameView 
      v-if="gameState === 'PLAYING'"
      :STAGES="STAGES"
      :stageIdx="stageIdx"
      :stage="stage"
      :bossHp="bossHp"
      :playerCards="playerCards"
      :playerScore="playerScore"
      :dealerCards="dealerCards"
      :dealerScore="dealerScore"
      :dealerRevealed="dealerRevealed"
      :playerName="playerName"
      :character="character"
      :characters="characters"
      :message="message"
      :phase="phase"
      :bossHit="bossHit"
      :showResult="showResult"
      @hit="doHit"
      @stay="doStay"
    />
    
  </div>
</template>

<style scoped>
</style>
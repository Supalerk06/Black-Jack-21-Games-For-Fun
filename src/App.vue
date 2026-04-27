<script setup>
import { ref } from 'vue'
import LobbyView from './components/LobbyView.vue'
import GameView from './components/GameView.vue'
import ResultView from './components/ResultView.vue'
import { computed } from 'vue'

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
</script>

<template>
  <div class="min-h-screen flex flex-col text-yellow-100 bg-[radial-gradient(ellipse_at_50%_30%,#2a1800_0%,#0d0900_70%,#050300_100%)]">
    </div>
</template>

<style scoped>
</style>
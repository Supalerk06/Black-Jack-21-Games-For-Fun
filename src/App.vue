<script setup>
import { ref, computed } from "vue";
import LobbyView from "./components/LobbyView.vue";
import GameView from "./components/GameView.vue";
import ResultView from "./components/ResultView.vue";

// Import boss assets
import boss1 from "./assets/boss1.webp";
import boss2 from "./assets/boss2.webp";
import boss3 from "./assets/boss3.webp";

// Import player assets
import player1 from "./assets/player1real.gif";
import player2 from "./assets/player2.webp";
import player3 from "./assets/player3.webp";

const STAGES = [
  {
    id: 1,
    name: "ATD Master",
    title: "First Boss",
    img: boss1,
    stopAt: 15,
    hp: 1,
    bg: "radial-gradient(ellipse at 50% 30%, #3d2800 0%, #1a0f00 60%, #0a0600 100%)",
    quote: '"เจ้ามีนามว่าอะไร !?"',
  },
  {
    id: 2,
    name: "Art Aud Aud Art ",
    title: "Second Boss",
    img: boss2,
    stopAt: 17,
    hp: 2,
    bg: "radial-gradient(ellipse at 50% 30%, #3d0000 0%, #1a0000 60%, #050000 100%)",
    quote: '"อื้ด อืดอื้อ อ้าด อ้าด"',
  },
  {
    id: 3,
    name: "The Reaper",
    title: "Last Boss",
    img: boss3,
    stopAt: 18,
    hp: 3,
    bg: "radial-gradient(ellipse at 50% 30%, #1a0030 0%, #0a0018 60%, #030008 100%)",
    quote: '"สวัดดี รู้มั้ยใครกำลังจะตาย...."',
  },
];
const characters = [
  { name: "มือปืนสี่ตา ", img: player1 },
  { name: "ค้อนยักบะลัก ", img: player2 },
  { name: "มีดคู่ดู๋ดี๋ ", img: player3 },
];

const gameState = ref("LOBBY");
const playerName = ref("");
const character = ref("");
const stageIdx = ref(0);
const playerCards = ref([]);
const dealerCards = ref([]);
const phase = ref("PLAYER");
const message = ref("");
const bossHp = ref(3);
const shake = ref(false);
const bossHit = ref(false);
const playerHit = ref(false);
const showResult = ref("");
const dealerRevealed = ref(false);
const showError = ref(false);

const stage = computed(() => STAGES[stageIdx.value]);
const rand = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min;
function makeCard(hidden = false) {
  return { val: rand(1, 10), hidden };
}
function calcScore(cards) {
  return cards.filter((c) => !c.hidden).reduce((a, c) => a + c.val, 0);
}
const playerScore = computed(() => calcScore(playerCards.value));
const dealerScore = computed(() => calcScore(dealerCards.value));

function startGame() {
  if (!playerName.value.trim() || !character.value) {
    showError.value = true;
    return;
  }
  showError.value = false;
  gameState.value = "PLAYING";
  bossHp.value = stage.value.hp;
  dealerRevealed.value = false;
  startRound();
}

function startRound() {
  playerCards.value = [makeCard(), makeCard()];
  dealerCards.value = [makeCard(), makeCard(true)];
  phase.value = "PLAYER";
  showResult.value = "";
  dealerRevealed.value = false;
  message.value = stage.value.quote;
  if (playerScore.value === 21) {
    message.value = "⚠️ แต้มสูงสุดแล้ว! (21) ควรหยุดจั่ว!";
  }
}

function doHit() {
  if (phase.value !== "PLAYER") return;
  playerCards.value.push(makeCard());
  if (playerScore.value > 21) {
    message.value = "⚡ พลังงานล้นระบบ! Core Overload!";
    triggerPlayerHit();
    endRound(false);
  } else if (playerScore.value === 21) {
    message.value = "⚠️ เพิ่มพลังงาน → 21 (แต้มสูงสุดแล้ว! ควรพอได้แล้ว)";
  } else {
    message.value = `เพิ่มพลังงาน → ${playerScore.value}`;
  }
}

async function doStay() {
  if (phase.value !== "PLAYER") return;
  phase.value = "DEALER";
  dealerCards.value = dealerCards.value.map((c) => ({ ...c, hidden: false }));
  dealerRevealed.value = true;
  message.value = `${stage.value.name} กำลังดำเนินการ...`;
  await delay(800);
  while (calcScore(dealerCards.value) < stage.value.stopAt) {
    dealerCards.value.push(makeCard());
    message.value = `${stage.value.name} เพิ่มพลัง → ${calcScore(dealerCards.value)}`;
    await delay(900);
  }
  const p = playerScore.value;
  const d = dealerScore.value;
  if (d > 21 || p > d) {
    message.value = "✦ ชัยชนะ! พลังงาน Aether ทำลายระบบ!";
    triggerBossHit();
    endRound(true);
  } else if (p < d) {
    message.value = "✸ ระบบล้มเหลว... ศัตรูมีพลังเหนือกว่า";
    triggerPlayerHit();
    endRound(false);
  } else {
    message.value = "◈ สมดุล! กำลังรีเซ็ตระบบ...";
    showResult.value = "DRAW";
    await delay(2000);
    startRound();
  }
}

async function endRound(isWin) {
  phase.value = "END";
  await delay(1200);
  showResult.value = isWin ? "WIN" : "LOSE";
  setTimeout(async () => {
    if (isWin) {
      bossHp.value--;
      if (bossHp.value <= 0) {
        if (stageIdx.value < STAGES.length - 1) {
          await delay(800);
          stageIdx.value++;
          bossHp.value = STAGES[stageIdx.value].hp;
          dealerRevealed.value = false;
          startRound();
        } else {
          gameState.value = "RESULT";
        }
      } else {
        startRound();
      }
    } else {
      startRound();
    }
  }, 2000);
}

function triggerBossHit() {
  bossHit.value = true;
  setTimeout(() => (bossHit.value = false), 600);
}
function triggerPlayerHit() {
  shake.value = true;
  playerHit.value = true;
  setTimeout(() => {
    shake.value = false;
    playerHit.value = false;
  }, 600);
}
const delay = (ms) => new Promise((r) => setTimeout(r, ms));
function restart() {
  stageIdx.value = 0;
  bossHp.value = STAGES[0].hp;
  playerName.value = "";
  character.value = "";
  gameState.value = "LOBBY";
}
</script>

<template>
  <div
    class="min-h-screen flex flex-col text-yellow-100 bg-[radial-gradient(ellipse_at_50%_30%,#2a1800_0%,#0d0900_70%,#050300_100%)]"
  >
    <LobbyView
      v-if="gameState === 'LOBBY'"
      v-model:playerName="playerName"
      v-model:character="character"
      :characters="characters"
      :showError="showError"
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
      @home="restart"
    />

    <ResultView
      v-if="gameState === 'RESULT'"
      :playerName="playerName"
      @restart="restart"
    />
  </div>
</template>

<style scoped></style>

# 🃏 BLACK JACK 21: BOSS BATTLE 🚀

Welcome to **Black Jack 21: Boss Battle**, a futuristic and exciting blackjack card game designed in a boss battle format! Developed using **Vue 3**, **Vite**, and **Tailwind CSS v4**, this project brings a unique and intense twist to the classic card game by turning it into a fight against three stage bosses. Test your luck, mathematical calculations, and strategic thinking!

---

## ✨ Key Features

*   **🎮 3-Stage Boss Battle System**: Face distinct bosses with unique health bars, quotes, and AI drawing behaviors (stop threshold):
    *   **Stage 1: ATD Master** (1 HP) - Beginner difficulty, stops drawing cards upon reaching 15. | *Greeting Quote: "What is your name!?"*
    *   **Stage 2: Art Aud Aud Art** (2 HP) - Intermediate difficulty, stops drawing cards upon reaching 17. | *Greeting Quote: "Ood Ood Oou Aad Aad"*
    *   **Stage 3: The Reaper** (3 HP) - Last boss difficulty, stops drawing cards upon reaching 18. | *Greeting Quote: "Hello, do you know who is about to die...."*
*   **👥 Playable Characters**:
    *   `มือปืนสี่ตา` (Four-eyed Gunslinger)
    *   `ค้อนยักบะลัก` (Gigantic Hammer Warrior)
    *   `มีดคู่ดู๋ดี๋` (Duo Hand Brawler)
*   **🪐 Premium Design & Animations (Rich Aesthetics)**:
    *   Sleek **Cyberpunk / Sci-fi** dark radial gradients and neon glow effects.
    *   Smooth card drawing animation float (`animate-card-draw`).
    *   Dynamic boss damage impact animations (`animate-boss-hit`) and screen-shake feedback when the player takes hits.
    *   Beautiful holographic **Result Pop-ups** and responsive gaming grid layouts.
*   **📖 Interactive Guide Book**: Displays clear gameplay rules inside the game, with smooth keyboard event listening (supports pressing `ESC` to close the guide and exit confirm modal).
*   **⚠️ Exit Confirmation Modal**: Safeguards the current stage progress and alerts players when trying to exit to the home menu mid-game.

---

## 🛠️ Tech Stack

*   **Core framework**: [Vue 3 (Composition API)](https://vuejs.org/) using `<script setup>` SFCs
*   **Styling**: [Tailwind CSS v4](https://tailwindcss.com/) with `@tailwindcss/vite` plugin for superior stylesheet processing and performance
*   **Bundler**: [Vite](https://vitejs.dev/) for extremely fast development and optimized build bundles

---

## 📂 Project Directory Structure

```bash
BlackJack-Games/
├── public/              # Folder for general static files
├── src/
│   ├── assets/          # Illustration assets for bosses, characters, and icons
│   │   ├── boss1.webp       # Stage 1 Boss (ATD Master)
│   │   ├── boss2.webp       # Stage 2 Boss (Art Aud Aud Art)
│   │   ├── boss3.webp       # Stage 3 Boss (The Reaper)
│   │   ├── player1real.gif  # Player 1 Character (Four-eyed Gunslinger)
│   │   ├── player2.webp     # Player 2 Character (Gigantic Hammer Warrior)
│   │   └── player3.webp     # Player 3 Character (Duo Hand Brawler)
│   ├── components/      # View components
│   │   ├── LobbyView.vue    # Player setup, character selection, and guide modal
│   │   ├── GameView.vue     # Core blackjack arena screen and controls
│   │   └── ResultView.vue   # Victory summary (Mission Complete) page
│   ├── App.vue          # Global state manager and game loop logic
│   ├── main.js          # Entry point for Vue app
│   └── style.css        # Tailwind directives and custom CSS keyframes
├── index.html           # Main index template
├── vite.config.js       # Vite configuration with Vue and Tailwind v4 plugins
└── package.json         # Package configuration and dependencies
```

---

## 📖 Basic Game Rules

1.  **Objective**: Accumulate cards in your hand to be as close to **21** as possible without exceeding it!
2.  **HIT**: Draw an extra card (randomly valued 1-10). If your score exceeds 21, a **Core Overload** occurs, and you lose the round immediately.
3.  **STAY**: Lock in your current score and let the boss take their turn to draw cards based on their specific AI threshold.
4.  **Defeating Bosses**:
    *   If the player scores higher than the boss, or if the boss busts (>21), the boss takes damage and loses **1 HP**.
    *   When the boss's HP drops to 0, the player advances to the next stage.
    *   Defeat all three bosses to achieve **Mission Complete** and win the game!

---

## 🚀 Getting Started

To run this project on your local machine, follow these steps:

### 1. Clone the Repository

```bash
git clone https://github.com/Supalerk06/Black-Jack-21-Games-For-Fun.git
cd Black-Jack-21-Games-For-Fun
```

### 2. Install Dependencies

Ensure you have Node.js installed, then run:

```bash
npm install
```

### 3. Run Development Server

Start the local development server:

```bash
npm run dev
```

Open the local address provided in your terminal (usually `http://localhost:5173`) in your browser to start playing!

### 4. Build for Production

Compile and optimize the code for production release:

```bash
npm run build
```

---

## 🤝 Contributors

Developed by students in the **CSS Framework** course. We welcome any feedback, issues, and feature requests for future updates! 🎮💡

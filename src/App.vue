<script setup lang="ts">
import { computed, ref } from 'vue'

type Flashcard = {
  japanese: string
  meaning: string
  kanjiReading: string
}

const cards: Flashcard[] = [
  { japanese: '勉強', meaning: 'Học tập', kanjiReading: 'べんきょう (benkyou)' },
  { japanese: '先生', meaning: 'Giáo viên', kanjiReading: 'せんせい (sensei)' },
  { japanese: '図書館', meaning: 'Thư viện', kanjiReading: 'としょかん (toshokan)' },
  { japanese: '友達', meaning: 'Bạn bè', kanjiReading: 'ともだち (tomodachi)' },
]

const currentIndex = ref(0)
const isFlipped = ref(false)

const currentCard = computed(() => cards[currentIndex.value])

function flipCard() {
  isFlipped.value = !isFlipped.value
}

function nextCard() {
  currentIndex.value = (currentIndex.value + 1) % cards.length
  isFlipped.value = false
}
</script>

<template>
  <main class="app">
    <h1 class="title">Học Tiếng Nhật cùng Toàn</h1>

    <div class="flashcard-scene" @click="flipCard" role="button" tabindex="0">
      <div class="flashcard" :class="{ flipped: isFlipped }">
        <section class="face front">
          <p class="label">Từ vựng tiếng Nhật</p>
          <p class="japanese">{{ currentCard.japanese }}</p>
          <p class="hint">Bấm vào thẻ để lật</p>
        </section>

        <section class="face back">
          <p class="label">Nghĩa tiếng Việt</p>
          <p class="meaning">{{ currentCard.meaning }}</p>
          <p class="reading">Cách đọc Kanji: {{ currentCard.kanjiReading }}</p>
        </section>
      </div>
    </div>

    <button class="next-btn" type="button" @click="nextCard">Tiếp theo</button>
  </main>
</template>

<style scoped>
.app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 1.5rem;
  padding: 2rem 1rem;
  background: linear-gradient(180deg, #f8fbff 0%, #eef3ff 100%);
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.title {
  margin: 0;
  text-align: center;
  font-size: clamp(1.8rem, 4vw, 2.6rem);
  color: #1f2937;
}

.flashcard-scene {
  width: min(92vw, 420px);
  height: 260px;
  perspective: 1000px;
  cursor: pointer;
}

.flashcard {
  position: relative;
  width: 100%;
  height: 100%;
  transform-style: preserve-3d;
  transition: transform 0.55s ease;
}

.flashcard.flipped {
  transform: rotateY(180deg);
}

.face {
  position: absolute;
  inset: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 0.75rem;
  border-radius: 16px;
  padding: 1.5rem;
  backface-visibility: hidden;
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.08);
}

.front {
  background: #ffffff;
  border: 1px solid #e5e7eb;
}

.back {
  background: #1f2937;
  color: #f9fafb;
  transform: rotateY(180deg);
}

.label {
  margin: 0;
  font-size: 0.9rem;
  opacity: 0.8;
}

.japanese {
  margin: 0;
  font-size: clamp(2rem, 8vw, 3rem);
  font-weight: 700;
  letter-spacing: 0.04em;
}

.meaning {
  margin: 0;
  font-size: 1.7rem;
  font-weight: 700;
}

.reading {
  margin: 0;
  text-align: center;
  font-size: 1rem;
  opacity: 0.95;
}

.hint {
  margin: 0;
  font-size: 0.86rem;
  color: #6b7280;
}

.next-btn {
  border: none;
  background: #2563eb;
  color: #ffffff;
  font-size: 1rem;
  font-weight: 600;
  padding: 0.75rem 1.5rem;
  border-radius: 10px;
  cursor: pointer;
  transition: background-color 0.2s ease, transform 0.2s ease;
}

.next-btn:hover {
  background: #1d4ed8;
  transform: translateY(-1px);
}

.next-btn:active {
  transform: translateY(0);
}
</style>

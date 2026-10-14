<script setup>
import { ref, onMounted, computed } from 'vue'

const journalEntries = ref({})
const showModal = ref(false)
const selectedEntry = ref({ date: '', entry: {} })

function openModal(date, entry) {
  selectedEntry.value = { date, entry }
  showModal.value = true
}

function closeModal() {
  showModal.value = false
  selectedEntry.value = { date: '', entry: {} }
}

onMounted(() => {
  const savedData = localStorage.getItem('journal_entries')
  if (savedData) {
    journalEntries.value = JSON.parse(savedData)
  }
})

const sortedJournals = computed(() => {
  const entriesArray = Object.entries(journalEntries.value)

  return entriesArray.sort((a, b) => {
    return b[0].localeCompare(a[0])
  })
})
</script>
<template>
  <div class="archive-container container">
    <h1>Arsip Jurnal</h1>
    <div v-if="sortedJournals.length === 0" class="empty-state">
      belum ada jurnal di cini, tulis dulu yuk! >_<
    </div>
    <div v-else class="journal-list">
      <div
        v-for="[date, entry] in sortedJournals"
        :key="date"
        class="journal-card"
        @click="openModal(date, entry)"
      >
        <div class="card-header">
          <span class="date">{{ date }}</span>
          <span class="mood" :class="'mood-' + entry.mood">{{ entry.mood }}</span>
        </div>
        <div class="card-body">
          <p>{{ entry.textJournal }}</p>
        </div>
      </div>
    </div>
    <Teleport to="body">
      <div v-if="showModal" class="modal-overlay" @click.self="closeModal">
        <div class="modal-content">
          <div class="modal-header">
            <span class="mood" :class="'mood-' + selectedEntry.mood">{{
              selectedEntry.entry.mood
            }}</span>
            <span class="date">{{ selectedEntry.date }}</span>
            <button class="close-button" @click="closeModal">X</button>
          </div>
          <div class="modal-body">
            <p>{{ selectedEntry.entry.textJournal }}</p>
          </div>
        </div>
      </div>
    </Teleport>
  </div>
</template>

<style scoped>
/* =============================
   DESIGN TOKENS
   ============================= */
.archive-container {
  display: flex;
  flex-direction: column;
  gap: 12px;
  overflow-y: auto;
}

/* =============================
   EMPTY STATE
   ============================= */
.empty-state {
  opacity: 0.3;
  text-align: center;
  border: 2px dashed #000;
  margin: auto;
  color: #000;
  background: var(--color-surface);
  padding: 12px 24px;
  border-radius: var(--radius-pill);
}

/* =============================
   JOURNAL LIST & CARD
   ============================= */
.journal-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.journal-card {
  display: flex;
  flex-direction: column;
  height: 130px;
  padding: 16px;
  box-sizing: border-box;
  background: var(--color-surface-alt);
  border-radius: var(--radius-sm);
  box-shadow: var(--shadow-sm);
  overflow: hidden;
  cursor: pointer;
  transition:
    transform 0.15s ease,
    box-shadow 0.15s ease;
}
.journal-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.12);
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-shrink: 0;
  margin-bottom: 8px;
  padding-bottom: 8px;
}

.card-body {
  flex: 1;
  min-height: 0;
  overflow: hidden;
  margin: 0;
  color: var(--color-text);
  line-height: 1.5;
}
.card-body p {
  margin: 0;
  white-space: pre-line;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis;
  word-break: break-word;
}

/* =============================
   BADGE: DATE & MOOD
   ============================= */
.date,
.mood {
  display: inline-flex;
  justify-content: center;
  align-items: center;
  padding: 6px 12px;
  border-radius: var(--radius-pill);
  text-transform: capitalize;
}

.date {
  font-weight: 700;
  background: var(--color-primary);
  color: #fff;
}

.mood {
  border: 1px solid;
}
.mood-sedih {
  background: var(--color-mood-sedih-bg);
  color: var(--color-mood-sedih-fg);
}
.mood-biasa {
  background: var(--color-mood-biasa-bg);
  color: var(--color-mood-biasa-fg);
}
.mood-senang {
  background: var(--color-primary-light);
  color: #006600;
}

/* =============================
   MODAL
   ============================= */
.modal-overlay {
  position: fixed;
  inset: 0; /* ganti top/left/right/bottom */
  display: flex;
  justify-content: center;
  align-items: center;
  background: rgba(0, 0, 0, 0.25);
  backdrop-filter: blur(5px);
  z-index: 999;
}

.modal-content {
  display: flex;
  flex-direction: column;
  gap: 6px;
  width: 90%;
  height: 90vh;
  padding: 12px;
  background: var(--color-surface);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-sm);
  /* ⬇ overflow dipindah ke .modal-body saja */
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 12px;
  border-radius: var(--radius-pill);
  font-size: 1rem;
}

/* Date di modal — versi outlined, bukan solid */
.modal-header .date {
  flex: 1;
  height: 52px; /* samakan dengan tinggi mood & close */
  background: transparent;
  color: var(--color-primary-dark);
  border: 1px solid;
}

/* Mood di modal — full color, kotak */
.modal-header .mood {
  width: 52px;
  height: 52px;
  font-size: 0.8rem;
  background: var(--color-primary);
  color: #fff;
  border: none;
}

/* Close button */
.close-button {
  width: 52px;
  height: 52px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 1.2rem;
  font-weight: 700;
  color: #fff;
  background: var(--color-primary);
  border: none;
  border-radius: var(--radius-pill);
  cursor: pointer;
  transition: background 0.15s ease;
}
.close-button:hover {
  background: var(--color-mood-sedih-fg); /* merah senada tema */
}

/* Body modal */
.modal-body {
  flex: 1;
  min-height: 0;
  padding: 12px;
  overflow-y: auto;
  color: var(--color-primary-dark);
  border: 1px solid var(--color-border);
  border-radius: 24px;
}
</style>

<script setup>
import { reactive, onMounted } from 'vue'

const getTodayDate = () => {
  const now = new Date()
  const day = String(now.getDate()).padStart(2, '0')
  const month = now.toLocaleString('id-ID', { month: 'long' })
  const year = now.getFullYear()
  return `${day}-${month}-${year}`
}

const todayNote = reactive({
  mood: 'biasa',
  textJournal: '',
})

onMounted(() => {
  const today = getTodayDate()
  const savedData = localStorage.getItem('journal_entries')

  if (savedData) {
    const allJournals = JSON.parse(savedData)

    if (allJournals[today]) {
      todayNote.mood = allJournals[today].mood
      todayNote.textJournal = allJournals[today].textJournal
    }
  }
})

const simpanJurnal = () => {
  const today = getTodayDate()

  const savedData = localStorage.getItem('journal_entries')
  const allJournals = savedData ? JSON.parse(savedData) : {}

  allJournals[today] = {
    mood: todayNote.mood,
    textJournal: todayNote.textJournal,
  }

  localStorage.setItem('journal_entries', JSON.stringify(allJournals))

  alert('Jurnal berhasil disimpan! Sugoi!')
}
</script>

<template>
  <div class="home-container container">
    <h1>Jurnal Hari Ini</h1>
    <h2>{{ getTodayDate() }}</h2>
    <form @submit.prevent="simpanJurnal">
      <div class="mood-field">
        <button
          type="button"
          class="mood-selector"
          :class="{ active: todayNote.mood === 'sedih' }"
          @click="todayNote.mood = 'sedih'"
        >
          😥 sedih
        </button>
        <button
          type="button"
          class="mood-selector"
          :class="{ active: todayNote.mood === 'biasa' }"
          @click="todayNote.mood = 'biasa'"
        >
          😐 biasa
        </button>
        <button
          type="button"
          class="mood-selector"
          :class="{ active: todayNote.mood === 'senang' }"
          @click="todayNote.mood = 'senang'"
        >
          😆 senang
        </button>
      </div>
      <textarea
        id="today-text-input"
        v-model="todayNote.textJournal"
        placeholder="Tulis jurnalmu hari ini. Jangan lupa klik simpan ya!"
      ></textarea>
      <button class="submit" type="submit">Simpan</button>
    </form>
  </div>
</template>

<style scoped>
form {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.mood-field {
  display: flex;
  gap: 12px;
}

.mood-field .mood-selector {
  flex: 1;
  padding: 8px 16px;
  font-size: 1rem;
  font-weight: bold;
  color: var(--color-primary);
  background: var(--color-surface);
  border: none;
  border-radius: var(--radius-sm);
  box-shadow: 0 0 30px 2px rgba(46, 125, 50, 0.19);
  cursor: pointer;
  transition: all 0.25s ease-in-out;
}

.mood-field .mood-selector:hover {
  border-radius: var(--radius-pill);
}

.mood-selector.active {
  color: var(--color-surface);
  background: var(--color-primary);
  border-radius: var(--radius-pill);
  box-shadow: var(--shadow-sm);
}

#today-text-input {
  flex: 1;
  padding: 12px;
  color: var(--color-primary);
  background: var(--color-surface-alt);
  border: none;
  border-radius: var(--radius-sm);
  box-shadow: 0 0 30px 2px rgba(46, 125, 50, 0.19);
  resize: none;
  outline: none;
}

.submit {
  padding: 12px 24px;
  font-size: 1rem;
  font-weight: bold;
  color: var(--color-surface);
  background: var(--color-primary);
  border: none;
  border-radius: var(--radius-pill);
  box-shadow: var(--shadow-sm);
  cursor: pointer;
  transition: all 0.25s ease-in-out;
}

.submit:hover {
  color: var(--color-primary-light);
  background: var(--color-primary-dark);
}
</style>

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
  border-radius: 12px;
  padding: 8px 16px;
  flex: 1;
  font-size: 1rem;
  font-weight: bold;
  border: 0px solid green;
  box-shadow: 0 0 30px 2px #00800030;
  background: white;
  transition: all 0.25s ease-in-out;
  color: green;
}
.mood-field .mood-selector:hover {
  border-radius: 99px;
}

.mood-selector.active {
  border-radius: 99px;
  background: green;
  border: 1px solid green;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  color: white;
}
#today-text-input {
  background: whitesmoke;
  border-radius: 16px;
  border: none;
  color: green;
  box-shadow: 0 0 30px 2px #00800030;
  resize: none;
  flex: 1;
  padding: 12px;
  outline: none;
}
.submit {
  padding: 12px 24px;
  border: none;
  font-size: 1rem;
  background: green;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: 99px;
  font-weight: bold;
  color: white;
  transition: all 0.25s ease-in-out;
}
.submit:hover {
  background: rgb(0, 81, 0);
  color: lightgreen;
}
</style>

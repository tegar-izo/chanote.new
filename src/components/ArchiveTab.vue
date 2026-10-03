<script setup>
import { ref, onMounted, computed } from 'vue'

const journalEntries = ref({})

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
      <div v-for="[date, entry] in sortedJournals" :key="date" class="journal-card">
        <div class="card-header">
          <span class="date">{{ date }}</span>
          <span class="mood" :class="'mood-' + entry.mood">{{ entry.mood }}</span>
        </div>
        <div class="card-body">
          <p style="white-space: pre-warp">{{ entry.textJournal }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.archive-container {
  gap: 12px;
  overflow-y: auto;
  /* background: #000; */
}
.empty-state {
  opacity: 30%;
  text-align: center;
  border: 2px dashed #000000;
  margin: auto;
  color: #000000;
  background: #ffffff;
  padding: 12px 24px;
  border-radius: 99px;
}
.journal-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
}
.journal-card {
  background: whitesmoke;
  padding: 16px;
  border-radius: 16px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 8px;
  padding-bottom: 8px;
}

.date {
  font-weight: bold;
  padding: 6px 12px;
  background: green;
  border-radius: 99px;
  color: white;
}
.mood {
  padding: 6px 12px;
  border-radius: 99px;
  border: 1px solid;
  /* font-size: 0.9rem; */
  text-transform: capitalize;
}
.mood-sedih {
  background: #ffcccc;
  color: #cc0000;
}
.mood-biasa {
  background: #e0e0e0;
  color: #333;
}
.mood-senang {
  background: #ccffcc;
  color: #006600;
}

.card-body {
  margin: 0;
  color: #444;
  line-height: 1.5;
}
</style>

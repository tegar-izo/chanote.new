<script setup>
import importIcon from '@/assets/import.svg'
import exportIcon from '@/assets/export.svg'
const exportData = () => {
  const savedData = localStorage.getItem('journal_entries')

  if (!savedData || savedData === '{}') {
    alert('Belum ada data jurnal untuk diekspor.')
    return
  }

  const blob = new Blob([savedData], { type: 'application/json' })
  const url = URL.createObjectURL(blob)

  const a = document.createElement('a')
  a.href = url

  const today = new Date().toISOString().slice(0, 10)
  a.download = `jurnal_backup_${today}.json`

  a.click()
  URL.revokeObjectURL(url)
}

const importData = () => {
  const input = document.createElement('input')
  input.type = 'file'
  input.accept = '.json'

  input.onchange = (event) => {
    const file = event.target.files[0]
    if (!file) return

    const reader = new FileReader()
    reader.onload = (e) => {
      try {
        const importedData = JSON.parse(e.target.result)

        const existingData = JSON.parse(localStorage.getItem('journal_entries') || '{}')

        const mergedData = { ...existingData, ...importedData }

        localStorage.setItem('journal_entries', JSON.stringify(mergedData))

        alert('Data berhasil diimpor! Silakan cek tab Archive.')
      } catch (error) {
        alert('Gagal mengimpor! Pastikan format file JSON valid.')
      }
    }

    reader.readAsText(file)
  }

  input.click()
}

const resetData = () => {
  const isConfirmed = confirm(
    'Apakah kamu yakin? Semua data jurnal akan terhapus dan TIDAK BISA KEMBALI',
  )

  if (isConfirmed) {
    localStorage.removeItem('journal_entries')
    alert('Semua data berhasil dihapus.')
  }
}
</script>

<template>
  <div class="settings-container container">
    <h1>settings</h1>

    <div class="settings-card">
      <h3>Data Management</h3>
      <button id="btn-export btn" @click="exportData">
        <exportIcon class="icon-style" /> export (JSON)
      </button>
      <button id="btn-import btn" @click="importData">
        <importIcon class="icon-style" /> import (JSON)
      </button>
    </div>

    <div class="settings-card">
      <h3>Reset Data</h3>
      <p>Hapus seluruh data jurnal dari perangkat ini. Tindakan ini tidak bisa dibatalkan.</p>
      <button id="btn-reset" class="btn-danger" @click="resetData">Hapus Semua Data</button>
    </div>
  </div>
</template>

<style scoped>
/* =============================
   CONTAINER
   ============================= */
.settings-container {
  display: flex;
  flex-direction: column;
  gap: 12px;
  padding: 12px;
  padding-bottom: 80px; /* ruang untuk navbar */
}

/* =============================
   CARD
   ============================= */
.settings-card {
  display: flex;
  flex-direction: column;
  gap: 12px;
  padding: 12px;
  background: var(--color-surface);
  border-radius: var(--radius-sm);
}

.settings-card h3 {
  margin: 0;
  padding: 6px;
  color: var(--color-primary);
  font-size: 1.3rem;
  border-bottom: 1px solid var(--color-primary);
}

.settings-card p {
  margin: 0;
  padding: 0 6px;
  color: var(--color-text);
}

/* =============================
   BUTTON (default = primary)
   ============================= */
.settings-card button {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 6px;
  padding: 12px;
  font-size: 1rem;
  font-weight: bold;
  color: var(--color-primary);
  background: var(--color-primary-light);
  border: 1px solid var(--color-primary);
  border-radius: var(--radius-pill);
  cursor: pointer;
  transition: all 0.25s ease-in-out;
}
.settings-card button:hover {
  color: var(--color-primary-light);
  background: var(--color-primary);
}

/* =============================
   BUTTON DANGER
   ============================= */
button.btn-danger {
  color: var(--color-danger-fg);
  background: var(--color-danger-bg);
  border: 1px solid var(--color-danger-fg);
}
button.btn-danger:hover {
  color: var(--color-danger-bg);
  background: var(--color-danger-fg);
  border-color: var(--color-danger-bg);
}
</style>

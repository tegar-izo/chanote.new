<script setup>
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
    'Apakah kamu yakin? Semua data jurnal akan terhapus dan TIDAK BISA KEMBALI (Yabai!).',
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
      <button id="btn-export" @click="exportData">Export Data (JSON)</button>
      <button id="btn-import" @click="importData">Import Data (JSON)</button>
    </div>

    <div class="settings-card">
      <h3>Reset Data</h3>
      <p>Hapus seluruh data jurnal dari perangkat ini. Tindakan ini tidak bisa dibatalkan.</p>
      <button id="btn-reset" class="btn-danger" @click="resetData">Hapus Semua Data</button>
    </div>
  </div>
</template>

<style scoped>
/* CSS tetap sama seperti milikmu */
.settings-container {
  padding: 12px;
  display: flex;
  flex-direction: column;
  gap: 12px;
  padding-bottom: 80px; /* Ruang untuk navbar */
}
.settings-card {
  background: white;
  padding: 12px;
  border-radius: 16px;
  display: flex;
  flex-direction: column;
  gap: 12px;
}
.settings-card h3 {
  padding: 6px;
  color: green;
  font-size: 1.3rem;
  border-bottom: 1px solid;
  margin: 0;
}
.settings-card p {
  padding: 0 6px;
  margin: 0;
  color: #444;
}
.settings-card button {
  cursor: pointer;
  padding: 12px;
  border-radius: 99px;
  font-size: 1rem;
  font-weight: bold;
  background: lightgreen;
  color: green;
  border: 1px solid green;
  transition: background 0.2s;
}
.settings-card button:hover {
  background: #a9dfa9;
}
button.btn-danger {
  background: #ffb4ab;
  color: #690005;
  border: 1px solid #690005;
}
button.btn-danger:hover {
  background: #ff9d92;
}
</style>

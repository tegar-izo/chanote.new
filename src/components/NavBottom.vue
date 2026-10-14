<script setup>
import homeIcon from '@/assets/home.svg'
import archiveIcon from '@/assets/archive.svg'
import settingsIcon from '@/assets/settings.svg'

const props = defineProps(['activeTab'])
const emit = defineEmits(['changeTab'])
</script>

<template>
  <div class="navbar">
    <button
      class="navbar-item"
      :class="{ active: props.activeTab === 'home' }"
      @click="emit('changeTab', 'home')"
    >
      <homeIcon class="icon-style" />
      home
    </button>

    <button
      class="navbar-item"
      :class="{ active: props.activeTab === 'archive' }"
      @click="emit('changeTab', 'archive')"
    >
      <archiveIcon class="icon-style" />
      archive
    </button>

    <button
      class="navbar-item"
      :class="{ active: props.activeTab === 'settings' }"
      @click="emit('changeTab', 'settings')"
    >
      <settingsIcon class="icon-style" />
      settings
    </button>
  </div>
</template>

<style scoped>
/* =============================
   NAVBAR
   ============================= */
.navbar {
  display: flex;
  position: sticky;
  width: fit-content;
  margin-bottom: 24px;
  padding: 6px;
  background: var(--color-primary-light);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-pill);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

/* =============================
   NAVBAR ITEM
   ============================= */
.navbar .navbar-item {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 3px;
  width: 100px;
  flex: 1;
  padding: 6px 12px;
  font-weight: bold;
  text-align: center;
  color: var(--color-primary);
  background: transparent;
  border: none;
  border-radius: var(--radius-pill);
  opacity: 0.6;
  cursor: pointer;
  transition: all 0.25s ease-in-out;
}

/* =============================
   ACTIVE STATE
   ============================= */
.navbar-item.active {
  color: var(--color-surface);
  background: var(--color-primary);
  opacity: 1;
  box-shadow: var(--shadow-sm);
}

/* =============================
   ICON
   ============================= */
.icon-style {
  width: 20px;
  height: 20px;
}
</style>

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
.navbar {
  background: lightgreen;
  display: flex;
  position: sticky;
  width: fit-content;
  margin-bottom: 24px;
  /* gap: 6px; */
  padding: 6px;
  border-radius: 99px;
  border: 1px solid #00000030;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

.navbar .navbar-item {
  width: 100px;
  flex: 1;
  text-align: center;
  border: none;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 3px;
  cursor: pointer;
  border-radius: 99px;
  background: transparent;
  padding: 6px 12px;
  color: green;
  opacity: 0.6;
  font-weight: bold;
  transition: all 0.25s ease-in-out;
}

.navbar-item.active {
  opacity: 1;
  background: green;
  color: #ffffffd0;

  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
</style>

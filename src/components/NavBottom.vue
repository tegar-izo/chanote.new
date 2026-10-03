<script setup>
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
      home
    </button>

    <button
      class="navbar-item"
      :class="{ active: props.activeTab === 'archive' }"
      @click="emit('changeTab', 'archive')"
    >
      archive
    </button>

    <button
      class="navbar-item"
      :class="{ active: props.activeTab === 'settings' }"
      @click="emit('changeTab', 'settings')"
    >
      settings
    </button>
  </div>
</template>

<style scoped>
.navbar {
  background: white;
  display: flex;
  position: sticky;
  width: fit-content;
  margin-bottom: 24px;
  /* gap: 6px; */
  padding: 6px;
  border-radius: 99px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

.navbar .navbar-item {
  width: 33%;
  text-align: center;
  border: none;
  cursor: pointer;
  border-radius: 99px;
  /* background: lightgreen; */
  background: transparent;
  padding: 12px 24px;
  color: green;
  font-weight: bold;
  transition: all 0.25s ease-in-out;
}
.navbar .navbar-item:hover {
  transform: scale(1.1);
}

.navbar-item.active {
  background: green;
  color: white;

  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
</style>

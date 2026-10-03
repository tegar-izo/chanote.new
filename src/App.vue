<script setup>
import { ref } from 'vue'
import HomeTab from './components/HomeTab.vue'
import ArchiveTab from './components/ArchiveTab.vue'
import SettingsTab from './components/SettingsTab.vue'
import NavBottom from './components/NavBottom.vue'

var activeTab = ref('home')
</script>
<template>
  <HomeTab v-if="activeTab === 'home'"></HomeTab>
  <ArchiveTab v-if="activeTab === 'archive'"></ArchiveTab>
  <SettingsTab v-if="activeTab === 'settings'"></SettingsTab>
  <NavBottom :activeTab="activeTab" @changeTab="activeTab = $event"></NavBottom>
</template>

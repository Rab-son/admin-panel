<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <router-view></router-view>
</template>

<style>
@import '@/assets/base.css';
@import '@/assets/main.css';

@tailwind base;
@tailwind components;
@tailwind utilities;
</style>

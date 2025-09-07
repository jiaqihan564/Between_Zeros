<script setup>
import BlogHomepage from './components/BlogHomepage.vue'
</script>

<template>
  <BlogHomepage />
</template>

<style>
/* 全局样式重置 */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
}

body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  line-height: 1.6;
  color: #333;
  overflow-x: hidden;
}

#app {
  min-height: 100vh;
}
</style>

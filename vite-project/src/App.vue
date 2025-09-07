<script setup>
import { ref, onMounted } from 'vue'
import BlogHomepage from './components/BlogHomepage.vue'
import DynamicBackground from './components/DynamicBackground.vue'
import DynamicCursor from './components/DynamicCursor.vue'
import ClickParticles from './components/ClickParticles.vue'

const clickParticles = ref(null)

// 处理鼠标点击事件
const handleClick = (e) => {
  // 防止在按钮和链接上触发粒子效果
  if (e.target.tagName === 'BUTTON' || e.target.tagName === 'A' || e.target.closest('button') || e.target.closest('a')) {
    return
  }
  
  if (clickParticles.value) {
    clickParticles.value.createClickParticles(e.clientX, e.clientY)
  }
}

onMounted(() => {
  document.addEventListener('click', handleClick)
})
</script>

<template>
  <div id="app">
    <!-- 动态背景 -->
    <DynamicBackground />
    
    <!-- 动态鼠标 -->
    <DynamicCursor />
    
    <!-- 点击粒子效果 -->
    <ClickParticles ref="clickParticles" />
    
    <!-- 主要内容 -->
    <BlogHomepage />
  </div>
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
  height: 100%;
  background: linear-gradient(
    135deg,
    #667eea 0%,
    #764ba2 25%,
    #f093fb 50%,
    #f5576c 75%,
    #4facfe 100%
  );
  background-attachment: fixed;
  background-repeat: no-repeat;
  background-size: cover;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  line-height: 1.6;
  color: #333;
  overflow-x: hidden;
  background: linear-gradient(
    135deg,
    #667eea 0%,
    #764ba2 25%,
    #f093fb 50%,
    #f5576c 75%,
    #4facfe 100%
  );
  background-attachment: fixed;
  background-repeat: no-repeat;
  background-size: cover;
  min-height: 100vh;
  margin: 0;
  padding: 0;
}

#app {
  min-height: 100vh;
  position: relative;
}

/* 确保内容在动态背景之上 */
#app > *:not(.dynamic-background):not(.dynamic-cursor):not(.click-particles) {
  position: relative;
  z-index: 1;
}

/* 移除分层效果，让内容直接显示在背景上 */
.blog-container {
  background: transparent;
  margin: 0;
  border: none;
  box-shadow: none;
}

/* 为按钮添加发光效果 */
.btn {
  position: relative;
  overflow: hidden;
  transition: all 0.3s ease;
}

.btn::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
  transition: left 0.5s;
}

.btn:hover::before {
  left: 100%;
}

/* 为卡片添加悬浮效果 */
.card, .project-card, .skill-item {
  transition: all 0.3s ease;
  background: transparent;
  border: none;
}

.card:hover, .project-card:hover, .skill-item:hover {
  transform: translateY(-10px);
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.2);
}

/* 文字发光效果和可读性 */
.hero-title, .section-title, h1, h2, h3, h4, h5, h6 {
  text-shadow: 0 0 20px rgba(255, 255, 255, 0.5), 0 0 10px rgba(0, 0, 0, 0.8);
}

p, span, div, a, li {
  text-shadow: 0 0 5px rgba(0, 0, 0, 0.7);
}

/* 导航栏透明化 */
.header {
  background: transparent;
  border: none;
}

/* 响应式调整 */
@media (max-width: 768px) {
  .blog-container {
    margin: 0;
  }
}
</style>

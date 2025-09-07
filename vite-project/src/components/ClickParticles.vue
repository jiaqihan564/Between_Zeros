<template>
  <div class="click-particles">
    <div 
      v-for="(particle, index) in particles" 
      :key="index"
      class="click-particle"
      :style="{
        left: particle.x + 'px',
        top: particle.y + 'px',
        background: particle.color,
        opacity: particle.life,
        transform: `translate(-50%, -50%) scale(${particle.life})`
      }"
    ></div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const particles = ref([])

// 创建点击粒子效果
const createClickParticles = (x, y) => {
  const colors = [
    '#ff6b6b', '#4ecdc4', '#45b7d1', '#96ceb4', 
    '#feca57', '#ff9ff3', '#a8e6cf', '#ffd3a5',
    '#fd79a8', '#6c5ce7', '#a29bfe', '#fd79a8'
  ]
  
  // 创建25个粒子
  for (let i = 0; i < 25; i++) {
    const angle = (Math.PI * 2 * i) / 25
    const velocity = Math.random() * 80 + 40
    const vx = Math.cos(angle) * velocity
    const vy = Math.sin(angle) * velocity
    
    particles.value.push({
      x: x,
      y: y,
      vx: vx,
      vy: vy,
      color: colors[Math.floor(Math.random() * colors.length)],
      life: 1,
      decay: 0.015
    })
  }
  
  // 动画粒子
  animateParticles()
}

// 动画粒子
const animateParticles = () => {
  const animate = () => {
    particles.value = particles.value.filter(particle => {
      // 更新位置
      particle.x += particle.vx * 0.016
      particle.y += particle.vy * 0.016
      
      // 应用重力
      particle.vy += 150 * 0.016
      
      // 应用空气阻力
      particle.vx *= 0.99
      particle.vy *= 0.99
      
      // 减少生命值
      particle.life -= particle.decay
      
      return particle.life > 0
    })
    
    if (particles.value.length > 0) {
      requestAnimationFrame(animate)
    }
  }
  
  animate()
}

// 暴露方法给父组件
defineExpose({
  createClickParticles
})
</script>

<style scoped>
.click-particles {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 10000;
}

.click-particle {
  position: absolute;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  box-shadow: 0 0 15px currentColor;
  transition: none;
}
</style>

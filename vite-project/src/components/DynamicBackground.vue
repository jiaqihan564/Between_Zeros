<template>
  <div class="dynamic-background" :style="{ height: backgroundHeight + 'px' }">
    <!-- 基础渐变背景层 -->
    <div class="gradient-bg"></div>
    
    <!-- 远山背景层 -->
    <div class="mountain-layer" :style="{ transform: `translateY(${scrollY * 0.1}px)` }">
      <div 
        v-for="(mountain, index) in mountains" 
        :key="'mountain-' + index"
        class="mountain"
        :style="{
          left: mountain.x + '%',
          height: mountain.height + 'px',
          background: mountain.color,
          transform: `translateY(${mountain.offset}px)`
        }"
      ></div>
    </div>
    
    <!-- 城市天际线层 -->
    <div class="city-skyline" :style="{ transform: `translateY(${scrollY * 0.12}px)` }">
      <div 
        v-for="(building, index) in buildings" 
        :key="'building-' + index"
        class="building"
        :style="{
          left: building.x + '%',
          height: building.height + 'px',
          width: building.width + 'px',
          background: building.color,
          animationDelay: building.delay + 's'
        }"
      ></div>
    </div>
    
    <!-- 云层背景 -->
    <div class="cloud-layer" :style="{ transform: `translateY(${scrollY * 0.2}px)` }">
      <div 
        v-for="(cloud, index) in clouds" 
        :key="'cloud-' + index"
        class="cloud"
        :style="{
          left: cloud.x + '%',
          top: cloud.y + '%',
          animationDelay: cloud.delay + 's',
          animationDuration: cloud.duration + 's'
        }"
      ></div>
    </div>
    
    <!-- 流星层 -->
    <div class="meteor-layer" :style="{ transform: `translateY(${scrollY * 0.3}px)` }">
      <div 
        v-for="(meteor, index) in meteors" 
        :key="'meteor-' + index"
        class="meteor"
        :style="{
          left: meteor.x + '%',
          top: meteor.y + '%',
          animationDelay: meteor.delay + 's',
          animationDuration: meteor.duration + 's'
        }"
      ></div>
    </div>
    
    <!-- 粒子背景 -->
    <canvas ref="particleCanvas" class="particle-canvas"></canvas>
    
    <!-- 星星背景 -->
    <div class="stars" :style="{ transform: `translateY(${scrollY * 0.5}px)` }">
      <div 
        v-for="(star, index) in stars" 
        :key="'star-' + index"
        class="star"
        :style="{
          left: star.x + '%',
          top: star.y + '%',
          animationDelay: star.delay + 's',
          animationDuration: star.duration + 's'
        }"
      ></div>
    </div>
    
    <!-- 浮动几何图形 -->
    <div class="floating-shapes" :style="{ transform: `translateY(${scrollY * 0.6}px)` }">
      <div 
        v-for="(shape, index) in shapes" 
        :key="index"
        :class="['shape', shape.type]"
        :style="{
          left: shape.x + '%',
          top: shape.y + '%',
          animationDelay: shape.delay + 's',
          animationDuration: shape.duration + 's'
        }"
      ></div>
    </div>
    
    
    <!-- 发光球体 -->
    <div class="glow-spheres" :style="{ transform: `translateY(${scrollY * 0.8}px)` }">
      <div 
        v-for="(sphere, index) in glowSpheres" 
        :key="'sphere-' + index"
        class="glow-sphere"
        :class="{ 'sphere-hover': sphere.isHovered }"
        :style="{
          left: sphere.x + '%',
          top: sphere.y + '%',
          animationDelay: sphere.delay + 's',
          animationDuration: sphere.duration + 's',
          background: sphere.color,
          boxShadow: `0 0 ${sphere.glow}px ${sphere.color}`,
          transform: `scale(${sphere.scale})`
        }"
        @mouseenter="handleSphereHover(index, true)"
        @mouseleave="handleSphereHover(index, false)"
      ></div>
    </div>
    
    <!-- 浮动文字 -->
    <div class="floating-text" :style="{ transform: `translateY(${scrollY * 0.9}px)` }">
      <div 
        v-for="(text, index) in floatingTexts" 
        :key="'text-' + index"
        class="floating-text-item"
        :style="{
          left: text.x + '%',
          top: text.y + '%',
          animationDelay: text.delay + 's',
          animationDuration: text.duration + 's',
          color: text.color
        }"
      >
        {{ text.content }}
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const particleCanvas = ref(null)
let animationId = null
let particles = []
let mouse = { x: 0, y: 0 }

// 滚动相关状态
const scrollY = ref(0)
const backgroundHeight = ref(window.innerHeight * 3) // 背景高度为视窗高度的3倍

// 浮动图形配置 - 优化分布和密度
const shapes = ref([
  { type: 'circle', x: 15, y: 25, delay: 0, duration: 12 },
  { type: 'triangle', x: 75, y: 15, delay: 2, duration: 15 },
  { type: 'square', x: 25, y: 65, delay: 4, duration: 10 },
  { type: 'circle', x: 65, y: 75, delay: 1, duration: 18 },
  { type: 'triangle', x: 85, y: 55, delay: 3, duration: 11 },
  { type: 'square', x: 10, y: 45, delay: 5, duration: 13 },
  { type: 'hexagon', x: 35, y: 35, delay: 6, duration: 16 },
  { type: 'diamond', x: 55, y: 50, delay: 7, duration: 14 },
  { type: 'circle', x: 80, y: 30, delay: 8, duration: 17 },
  { type: 'triangle', x: 20, y: 80, delay: 9, duration: 12 },
  { type: 'square', x: 45, y: 20, delay: 10, duration: 9 },
  { type: 'hexagon', x: 70, y: 40, delay: 11, duration: 13 }
])

// 星星配置 - 优化分布和数量
const stars = ref([
  { x: 8, y: 12, delay: 0, duration: 3.5 },
  { x: 22, y: 6, delay: 0.3, duration: 4.2 },
  { x: 38, y: 18, delay: 0.6, duration: 2.8 },
  { x: 52, y: 10, delay: 0.9, duration: 3.8 },
  { x: 68, y: 15, delay: 1.2, duration: 4.5 },
  { x: 82, y: 8, delay: 1.5, duration: 3.2 },
  { x: 95, y: 22, delay: 1.8, duration: 2.5 },
  { x: 12, y: 28, delay: 2.1, duration: 4.1 },
  { x: 28, y: 35, delay: 2.4, duration: 3.6 },
  { x: 45, y: 42, delay: 2.7, duration: 2.9 },
  { x: 62, y: 38, delay: 3.0, duration: 4.3 },
  { x: 78, y: 45, delay: 3.3, duration: 3.4 },
  { x: 92, y: 32, delay: 3.6, duration: 2.7 },
  { x: 6, y: 55, delay: 3.9, duration: 4.0 },
  { x: 24, y: 62, delay: 4.2, duration: 3.1 },
  { x: 42, y: 58, delay: 4.5, duration: 2.6 },
  { x: 58, y: 65, delay: 4.8, duration: 3.9 },
  { x: 74, y: 72, delay: 5.1, duration: 3.3 },
  { x: 88, y: 68, delay: 5.4, duration: 2.8 },
  { x: 15, y: 78, delay: 5.7, duration: 4.4 },
  { x: 35, y: 82, delay: 6.0, duration: 3.7 },
  { x: 55, y: 75, delay: 6.3, duration: 2.4 },
  { x: 72, y: 85, delay: 6.6, duration: 3.8 },
  { x: 90, y: 78, delay: 6.9, duration: 3.0 }
])


// 发光球体配置
const glowSpheres = ref([
  { x: 20, y: 25, delay: 0, duration: 12, color: '#ff6b6b', glow: 30, isHovered: false, scale: 1 },
  { x: 60, y: 15, delay: 3, duration: 15, color: '#4ecdc4', glow: 25, isHovered: false, scale: 1 },
  { x: 80, y: 45, delay: 6, duration: 18, color: '#45b7d1', glow: 35, isHovered: false, scale: 1 },
  { x: 30, y: 65, delay: 9, duration: 14, color: '#96ceb4', glow: 28, isHovered: false, scale: 1 },
  { x: 70, y: 80, delay: 12, duration: 16, color: '#feca57', glow: 32, isHovered: false, scale: 1 },
  { x: 10, y: 50, delay: 15, duration: 13, color: '#ff9ff3', glow: 26, isHovered: false, scale: 1 }
])

// 浮动文字配置
const floatingTexts = ref([
  { x: 15, y: 20, delay: 0, duration: 25, content: '✨', color: '#ffd700' },
  { x: 85, y: 30, delay: 5, duration: 30, content: '💫', color: '#87ceeb' },
  { x: 25, y: 60, delay: 10, duration: 28, content: '🌟', color: '#ff69b4' },
  { x: 75, y: 70, delay: 15, duration: 22, content: '⭐', color: '#98fb98' },
  { x: 45, y: 85, delay: 20, duration: 26, content: '🌙', color: '#dda0dd' },
  { x: 90, y: 15, delay: 25, duration: 24, content: '☀️', color: '#ffa500' }
])

// 远山配置
const mountains = ref([
  { x: 0, height: 200, color: 'linear-gradient(180deg, #2c3e50, #34495e)', offset: 0 },
  { x: 20, height: 250, color: 'linear-gradient(180deg, #34495e, #2c3e50)', offset: 20 },
  { x: 40, height: 180, color: 'linear-gradient(180deg, #2c3e50, #34495e)', offset: -10 },
  { x: 60, height: 300, color: 'linear-gradient(180deg, #34495e, #2c3e50)', offset: 30 },
  { x: 80, height: 220, color: 'linear-gradient(180deg, #2c3e50, #34495e)', offset: -5 }
])

// 云层配置
const clouds = ref([
  { x: 10, y: 15, delay: 0, duration: 20 },
  { x: 30, y: 25, delay: 5, duration: 25 },
  { x: 50, y: 10, delay: 10, duration: 18 },
  { x: 70, y: 20, delay: 15, duration: 22 },
  { x: 90, y: 30, delay: 20, duration: 16 },
  { x: 15, y: 40, delay: 25, duration: 24 },
  { x: 35, y: 50, delay: 30, duration: 19 },
  { x: 55, y: 35, delay: 35, duration: 21 },
  { x: 75, y: 45, delay: 40, duration: 17 },
  { x: 95, y: 55, delay: 45, duration: 23 }
])

// 城市天际线配置
const buildings = ref([
  { x: 0, height: 120, width: 15, color: 'linear-gradient(180deg, #2c3e50, #34495e)', delay: 0 },
  { x: 15, height: 80, width: 12, color: 'linear-gradient(180deg, #34495e, #2c3e50)', delay: 1 },
  { x: 27, height: 150, width: 18, color: 'linear-gradient(180deg, #2c3e50, #34495e)', delay: 2 },
  { x: 45, height: 100, width: 14, color: 'linear-gradient(180deg, #34495e, #2c3e50)', delay: 3 },
  { x: 59, height: 200, width: 20, color: 'linear-gradient(180deg, #2c3e50, #34495e)', delay: 4 },
  { x: 79, height: 90, width: 16, color: 'linear-gradient(180deg, #34495e, #2c3e50)', delay: 5 },
  { x: 95, height: 110, width: 5, color: 'linear-gradient(180deg, #2c3e50, #34495e)', delay: 6 }
])

// 流星配置
const meteors = ref([
  { x: 10, y: 10, delay: 0, duration: 3 },
  { x: 30, y: 20, delay: 2, duration: 4 },
  { x: 50, y: 15, delay: 4, duration: 3.5 },
  { x: 70, y: 25, delay: 6, duration: 2.8 },
  { x: 90, y: 12, delay: 8, duration: 3.2 },
  { x: 20, y: 35, delay: 10, duration: 4.5 },
  { x: 40, y: 40, delay: 12, duration: 3.8 },
  { x: 60, y: 30, delay: 14, duration: 2.5 },
  { x: 80, y: 45, delay: 16, duration: 3.6 },
  { x: 5, y: 50, delay: 18, duration: 4.2 }
])

// 粒子类
class Particle {
  constructor(canvas) {
    this.canvas = canvas
    this.x = Math.random() * canvas.width
    this.y = Math.random() * canvas.height
    this.vx = (Math.random() - 0.5) * 0.5
    this.vy = (Math.random() - 0.5) * 0.5
    this.size = Math.random() * 2 + 1
    this.opacity = Math.random() * 0.5 + 0.2
  }

  update() {
    this.x += this.vx
    this.y += this.vy

    // 边界检测
    if (this.x < 0 || this.x > this.canvas.width) this.vx *= -1
    if (this.y < 0 || this.y > this.canvas.height) this.vy *= -1

    // 移除鼠标交互，让粒子保持自然运动
  }

  draw(ctx) {
    ctx.beginPath()
    ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2)
    ctx.fillStyle = `rgba(255, 255, 255, ${this.opacity})`
    ctx.fill()
  }
}

// 初始化粒子
const initParticles = () => {
  const canvas = particleCanvas.value
  if (!canvas) return
  
  const ctx = canvas.getContext('2d')
  canvas.width = window.innerWidth
  canvas.height = window.innerHeight
  
  particles = []
  for (let i = 0; i < 100; i++) {
    particles.push(new Particle(canvas))
  }
}

// 动画循环
const animate = () => {
  const canvas = particleCanvas.value
  if (!canvas) return
  
  const ctx = canvas.getContext('2d')
  ctx.clearRect(0, 0, canvas.width, canvas.height)
  
  particles.forEach(particle => {
    particle.update()
    particle.draw(ctx)
  })
  
  // 绘制连接线
  particles.forEach((particle, i) => {
    particles.slice(i + 1).forEach(otherParticle => {
      const dx = particle.x - otherParticle.x
      const dy = particle.y - otherParticle.y
      const distance = Math.sqrt(dx * dx + dy * dy)
      
      if (distance < 100) {
        ctx.beginPath()
        ctx.moveTo(particle.x, particle.y)
        ctx.lineTo(otherParticle.x, otherParticle.y)
        ctx.strokeStyle = `rgba(255, 255, 255, ${0.1 * (1 - distance / 100)})`
        ctx.lineWidth = 1
        ctx.stroke()
      }
    })
  })
  
  animationId = requestAnimationFrame(animate)
}

// 移除鼠标移动事件，不再需要鼠标交互


// 发光球体悬停处理
const handleSphereHover = (index, isHovered) => {
  glowSpheres.value[index].isHovered = isHovered
  if (isHovered) {
    // 悬停时放大球体
    glowSpheres.value[index].scale = 1.5
    glowSpheres.value[index].glow = glowSpheres.value[index].glow * 1.5
  } else {
    glowSpheres.value[index].scale = 1
    glowSpheres.value[index].glow = glowSpheres.value[index].glow / 1.5
  }
}

// 滚动事件处理（添加节流优化）
let scrollTimeout = null
const handleScroll = () => {
  if (scrollTimeout) return
  scrollTimeout = setTimeout(() => {
    scrollY.value = window.scrollY
    scrollTimeout = null
  }, 16) // 约60fps
}

// 窗口大小改变事件
const handleResize = () => {
  const canvas = particleCanvas.value
  if (!canvas) return
  
  canvas.width = window.innerWidth
  canvas.height = window.innerHeight
  backgroundHeight.value = window.innerHeight * 3
}

onMounted(() => {
  initParticles()
  animate()
  window.addEventListener('resize', handleResize)
  window.addEventListener('scroll', handleScroll)
})

onUnmounted(() => {
  if (animationId) {
    cancelAnimationFrame(animationId)
  }
  window.removeEventListener('resize', handleResize)
  window.removeEventListener('scroll', handleScroll)
})
</script>

<style scoped>
.dynamic-background {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  min-height: 100vh;
  z-index: -1;
  overflow: hidden;
  will-change: transform;
  margin: 0;
  padding: 0;
}

.particle-canvas {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.gradient-bg {
  position: absolute;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  min-height: 100vh;
  background: linear-gradient(
    135deg,
    #667eea 0%,
    #764ba2 25%,
    #f093fb 50%,
    #f5576c 75%,
    #4facfe 100%
  );
  background-size: 400% 400%;
  background-attachment: fixed;
  background-repeat: no-repeat;
  background-position: center center;
  animation: gradientShift 15s ease infinite;
  margin: 0;
  padding: 0;
}

@keyframes gradientShift {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}

.floating-shapes {
  position: absolute;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  min-height: 100vh;
  pointer-events: none;
}

.shape {
  position: absolute;
  opacity: 0.08;
  animation: float infinite ease-in-out;
  filter: blur(0.5px);
}

.shape.circle {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  background: linear-gradient(45deg, #ff6b6b, #4ecdc4);
}

.shape.triangle {
  width: 0;
  height: 0;
  border-left: 30px solid transparent;
  border-right: 30px solid transparent;
  border-bottom: 52px solid #ff6b6b;
}

.shape.square {
  width: 50px;
  height: 50px;
  background: linear-gradient(45deg, #4ecdc4, #45b7d1);
  transform: rotate(45deg);
}

.shape.hexagon {
  width: 40px;
  height: 40px;
  background: linear-gradient(45deg, #ff9ff3, #f368e0);
  clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
}

.shape.diamond {
  width: 35px;
  height: 35px;
  background: linear-gradient(45deg, #feca57, #ff9f43);
  transform: rotate(45deg);
}

@keyframes float {
  0%, 100% {
    transform: translateY(0px) rotate(0deg) scale(1);
  }
  25% {
    transform: translateY(-15px) rotate(90deg) scale(1.05);
  }
  50% {
    transform: translateY(-30px) rotate(180deg) scale(1.1);
  }
  75% {
    transform: translateY(-15px) rotate(270deg) scale(1.05);
  }
}

/* 星星样式 */
.stars {
  position: absolute;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  min-height: 100vh;
  pointer-events: none;
}

.star {
  position: absolute;
  width: 3px;
  height: 3px;
  background: #fff;
  border-radius: 50%;
  animation: twinkle infinite ease-in-out;
  box-shadow: 0 0 8px rgba(255, 255, 255, 0.6);
  filter: blur(0.3px);
}

@keyframes twinkle {
  0%, 100% { opacity: 0.2; transform: scale(1); }
  50% { opacity: 0.8; transform: scale(1.3); }
}


/* 发光球体样式 */
.glow-spheres {
  position: absolute;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  min-height: 100vh;
  pointer-events: none;
}

.glow-sphere {
  position: absolute;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  animation: pulse infinite ease-in-out;
  cursor: pointer;
  transition: all 0.3s ease;
}

.sphere-hover {
  animation: pulseHover infinite ease-in-out;
}

@keyframes pulse {
  0%, 100% { transform: scale(1); opacity: 0.6; }
  50% { transform: scale(1.5); opacity: 0.8; }
}

@keyframes pulseHover {
  0%, 100% { transform: scale(1.5); opacity: 0.9; }
  50% { transform: scale(2); opacity: 1; }
}

/* 浮动文字样式 */
.floating-text {
  position: absolute;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  min-height: 100vh;
  pointer-events: none;
}

.floating-text-item {
  position: absolute;
  font-size: 2rem;
  animation: floatText infinite ease-in-out;
  text-shadow: 0 0 10px currentColor;
}

@keyframes floatText {
  0%, 100% { 
    transform: translateY(0px) rotate(0deg) scale(1); 
    opacity: 0.7;
  }
  25% { 
    transform: translateY(-15px) rotate(90deg) scale(1.1); 
    opacity: 1;
  }
  50% { 
    transform: translateY(-30px) rotate(180deg) scale(0.9); 
    opacity: 0.8;
  }
  75% { 
    transform: translateY(-15px) rotate(270deg) scale(1.1); 
    opacity: 1;
  }
}

/* 远山层样式 */
.mountain-layer {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  min-height: 100vh;
  pointer-events: none;
  will-change: transform;
}

.mountain {
  position: absolute;
  bottom: 0;
  width: 100%;
  border-radius: 50% 50% 0 0;
  opacity: 0.15;
  filter: blur(2px);
}

/* 云层样式 */
.cloud-layer {
  position: absolute;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  min-height: 100vh;
  pointer-events: none;
  will-change: transform;
}

.cloud {
  position: absolute;
  width: 100px;
  height: 50px;
  background: rgba(255, 255, 255, 0.08);
  border-radius: 50px;
  animation: cloudFloat infinite ease-in-out;
  opacity: 0.4;
  filter: blur(1px);
}

.cloud::before {
  content: '';
  position: absolute;
  top: -25px;
  left: 15px;
  width: 60px;
  height: 60px;
  background: rgba(255, 255, 255, 0.06);
  border-radius: 50%;
}

.cloud::after {
  content: '';
  position: absolute;
  top: -20px;
  right: 15px;
  width: 50px;
  height: 50px;
  background: rgba(255, 255, 255, 0.06);
  border-radius: 50%;
}

@keyframes cloudFloat {
  0%, 100% { transform: translateX(0px); }
  50% { transform: translateX(20px); }
}

/* 城市天际线样式 */
.city-skyline {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  min-height: 100vh;
  pointer-events: none;
}

.building {
  position: absolute;
  bottom: 0;
  opacity: 0.12;
  animation: buildingGlow infinite ease-in-out;
  filter: blur(1.5px);
}

@keyframes buildingGlow {
  0%, 100% { opacity: 0.2; }
  50% { opacity: 0.4; }
}

/* 流星层样式 */
.meteor-layer {
  position: absolute;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  min-height: 100vh;
  pointer-events: none;
}

.meteor {
  position: absolute;
  width: 2px;
  height: 2px;
  background: #fff;
  border-radius: 50%;
  animation: meteorFall infinite linear;
  box-shadow: 0 0 10px #fff, 0 0 20px #fff, 0 0 30px #fff;
}

.meteor::before {
  content: '';
  position: absolute;
  top: 0;
  left: -20px;
  width: 20px;
  height: 1px;
  background: linear-gradient(90deg, transparent, #fff);
  border-radius: 1px;
}

@keyframes meteorFall {
  0% { 
    transform: translateY(-100px) translateX(0px);
    opacity: 0;
  }
  10% { 
    opacity: 1;
  }
  90% { 
    opacity: 1;
  }
  100% { 
    transform: translateY(100vh) translateX(50px);
    opacity: 0;
  }
}

/* 响应式设计 */
@media (max-width: 768px) {
  .shape {
    opacity: 0.04;
  }
  
  .shape.circle,
  .shape.square {
    width: 35px;
    height: 35px;
  }
  
  .shape.triangle {
    border-left: 18px solid transparent;
    border-right: 18px solid transparent;
    border-bottom: 30px solid #ff6b6b;
  }
  
  .star {
    width: 2px;
    height: 2px;
  }
  
  .glow-sphere {
    width: 12px;
    height: 12px;
  }
  
  .floating-text-item {
    font-size: 1.2rem;
  }
  
  .cloud {
    width: 70px;
    height: 35px;
  }
  
  .mountain {
    opacity: 0.1;
  }
  
  .building {
    opacity: 0.08;
  }
  
  
  .meteor {
    width: 1px;
    height: 1px;
  }
}

@media (max-width: 480px) {
  .shape {
    opacity: 0.03;
  }
  
  .shape.circle,
  .shape.square {
    width: 30px;
    height: 30px;
  }
  
  .star {
    width: 1.5px;
    height: 1.5px;
  }
  
  .floating-text-item {
    font-size: 1rem;
  }
  
  .cloud {
    width: 60px;
    height: 30px;
  }
}
</style>

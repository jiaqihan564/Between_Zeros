<template>
  <div class="dynamic-background">
    <!-- 粒子背景 -->
    <canvas ref="particleCanvas" class="particle-canvas"></canvas>
    
    <!-- 渐变背景 -->
    <div class="gradient-bg"></div>
    
    <!-- 浮动几何图形 -->
    <div class="floating-shapes">
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
    
    <!-- 星星背景 -->
    <div class="stars">
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
    
    <!-- 彩虹光带 -->
    <div class="rainbow-strips">
      <div 
        v-for="(strip, index) in rainbowStrips" 
        :key="'strip-' + index"
        class="rainbow-strip"
        :class="{ 'strip-hover': strip.isHovered }"
        :style="{
          left: strip.x + '%',
          top: strip.y + '%',
          animationDelay: strip.delay + 's',
          animationDuration: strip.duration + 's',
          background: strip.color,
          transform: `translateX(${strip.offset}px)`
        }"
        @mouseenter="handleStripHover(index, true)"
        @mouseleave="handleStripHover(index, false)"
      ></div>
    </div>
    
    <!-- 发光球体 -->
    <div class="glow-spheres">
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
    <div class="floating-text">
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

// 浮动图形配置
const shapes = ref([
  { type: 'circle', x: 10, y: 20, delay: 0, duration: 8 },
  { type: 'triangle', x: 80, y: 10, delay: 2, duration: 12 },
  { type: 'square', x: 20, y: 70, delay: 4, duration: 10 },
  { type: 'circle', x: 70, y: 80, delay: 1, duration: 15 },
  { type: 'triangle', x: 90, y: 60, delay: 3, duration: 9 },
  { type: 'square', x: 5, y: 50, delay: 5, duration: 11 },
  { type: 'hexagon', x: 30, y: 30, delay: 6, duration: 14 },
  { type: 'diamond', x: 60, y: 40, delay: 7, duration: 13 },
  { type: 'circle', x: 85, y: 25, delay: 8, duration: 16 },
  { type: 'triangle', x: 15, y: 85, delay: 9, duration: 11 }
])

// 星星配置
const stars = ref([
  { x: 5, y: 15, delay: 0, duration: 3 },
  { x: 25, y: 8, delay: 0.5, duration: 4 },
  { x: 45, y: 20, delay: 1, duration: 2.5 },
  { x: 65, y: 12, delay: 1.5, duration: 3.5 },
  { x: 85, y: 18, delay: 2, duration: 4.5 },
  { x: 15, y: 35, delay: 2.5, duration: 3 },
  { x: 35, y: 45, delay: 3, duration: 2.8 },
  { x: 55, y: 38, delay: 3.5, duration: 4.2 },
  { x: 75, y: 42, delay: 4, duration: 3.8 },
  { x: 95, y: 35, delay: 4.5, duration: 2.2 },
  { x: 8, y: 65, delay: 5, duration: 3.2 },
  { x: 28, y: 72, delay: 5.5, duration: 4.8 },
  { x: 48, y: 68, delay: 6, duration: 2.8 },
  { x: 68, y: 75, delay: 6.5, duration: 3.6 },
  { x: 88, y: 68, delay: 7, duration: 4.1 }
])

// 彩虹光带配置
const rainbowStrips = ref([
  { x: 0, y: 10, delay: 0, duration: 20, color: 'linear-gradient(90deg, #ff6b6b, #4ecdc4)', isHovered: false, offset: 0 },
  { x: 0, y: 30, delay: 5, duration: 25, color: 'linear-gradient(90deg, #4ecdc4, #45b7d1)', isHovered: false, offset: 0 },
  { x: 0, y: 50, delay: 10, duration: 18, color: 'linear-gradient(90deg, #45b7d1, #96ceb4)', isHovered: false, offset: 0 },
  { x: 0, y: 70, delay: 15, duration: 22, color: 'linear-gradient(90deg, #96ceb4, #feca57)', isHovered: false, offset: 0 },
  { x: 0, y: 90, delay: 20, duration: 16, color: 'linear-gradient(90deg, #feca57, #ff9ff3)', isHovered: false, offset: 0 }
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

    // 鼠标交互
    const dx = mouse.x - this.x
    const dy = mouse.y - this.y
    const distance = Math.sqrt(dx * dx + dy * dy)
    
    if (distance < 100) {
      const force = (100 - distance) / 100
      this.x -= dx * force * 0.01
      this.y -= dy * force * 0.01
    }
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

// 鼠标移动事件
const handleMouseMove = (e) => {
  mouse.x = e.clientX
  mouse.y = e.clientY
}

// 彩虹光带悬停处理
const handleStripHover = (index, isHovered) => {
  rainbowStrips.value[index].isHovered = isHovered
  if (isHovered) {
    // 悬停时增加偏移量，产生碰撞效果
    rainbowStrips.value[index].offset = Math.random() * 20 - 10
  } else {
    rainbowStrips.value[index].offset = 0
  }
}

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

// 窗口大小改变事件
const handleResize = () => {
  const canvas = particleCanvas.value
  if (!canvas) return
  
  canvas.width = window.innerWidth
  canvas.height = window.innerHeight
}

onMounted(() => {
  initParticles()
  animate()
  window.addEventListener('mousemove', handleMouseMove)
  window.addEventListener('resize', handleResize)
})

onUnmounted(() => {
  if (animationId) {
    cancelAnimationFrame(animationId)
  }
  window.removeEventListener('mousemove', handleMouseMove)
  window.removeEventListener('resize', handleResize)
})
</script>

<style scoped>
.dynamic-background {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
  overflow: hidden;
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
  width: 100%;
  height: 100%;
  background: linear-gradient(
    135deg,
    #667eea 0%,
    #764ba2 25%,
    #f093fb 50%,
    #f5576c 75%,
    #4facfe 100%
  );
  background-size: 400% 400%;
  animation: gradientShift 15s ease infinite;
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
  width: 100%;
  height: 100%;
  pointer-events: none;
}

.shape {
  position: absolute;
  opacity: 0.1;
  animation: float infinite ease-in-out;
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
    transform: translateY(0px) rotate(0deg);
  }
  25% {
    transform: translateY(-20px) rotate(90deg);
  }
  50% {
    transform: translateY(-40px) rotate(180deg);
  }
  75% {
    transform: translateY(-20px) rotate(270deg);
  }
}

/* 星星样式 */
.stars {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
}

.star {
  position: absolute;
  width: 4px;
  height: 4px;
  background: #fff;
  border-radius: 50%;
  animation: twinkle infinite ease-in-out;
  box-shadow: 0 0 6px rgba(255, 255, 255, 0.8);
}

@keyframes twinkle {
  0%, 100% { opacity: 0.3; transform: scale(1); }
  50% { opacity: 1; transform: scale(1.2); }
}

/* 彩虹光带样式 */
.rainbow-strips {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
}

.rainbow-strip {
  position: absolute;
  width: 100%;
  height: 8px;
  opacity: 0.4;
  animation: slideAcross infinite linear;
  cursor: pointer;
  transition: all 0.3s ease;
  border-radius: 4px;
  box-shadow: 0 0 10px rgba(255, 255, 255, 0.3);
}

.strip-hover {
  height: 12px;
  opacity: 0.8;
  box-shadow: 0 0 20px rgba(255, 255, 255, 0.6);
  transform: scaleY(1.5);
}

@keyframes slideAcross {
  0% { transform: translateX(-100%); }
  100% { transform: translateX(100%); }
}

/* 发光球体样式 */
.glow-spheres {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
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
  width: 100%;
  height: 100%;
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

/* 响应式设计 */
@media (max-width: 768px) {
  .shape {
    opacity: 0.05;
  }
  
  .shape.circle,
  .shape.square {
    width: 40px;
    height: 40px;
  }
  
  .shape.triangle {
    border-left: 20px solid transparent;
    border-right: 20px solid transparent;
    border-bottom: 35px solid #ff6b6b;
  }
  
  .star {
    width: 3px;
    height: 3px;
  }
  
  .glow-sphere {
    width: 15px;
    height: 15px;
  }
  
  .floating-text-item {
    font-size: 1.5rem;
  }
}
</style>

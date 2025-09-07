<template>
  <div class="dynamic-cursor">
    <!-- 主光标 -->
    <div 
      ref="cursor" 
      class="cursor"
      :class="{ 'cursor-hover': isHovering, 'cursor-click': isClicking }"
    ></div>
    
    <!-- 光标跟随点 -->
    <div 
      ref="cursorFollower" 
      class="cursor-follower"
      :class="{ 'follower-hover': isHovering }"
    ></div>
    
    <!-- 光标光环 -->
    <div 
      ref="cursorRing" 
      class="cursor-ring"
      :class="{ 'ring-hover': isHovering }"
    ></div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const cursor = ref(null)
const cursorFollower = ref(null)
const cursorRing = ref(null)

let mouseX = 0
let mouseY = 0
let followerX = 0
let followerY = 0
let ringX = 0
let ringY = 0
let isHovering = ref(false)
let isClicking = ref(false)
let animationId = null

// 鼠标移动事件
const handleMouseMove = (e) => {
  mouseX = e.clientX
  mouseY = e.clientY
  
  // 更新主光标位置
  if (cursor.value) {
    cursor.value.style.left = mouseX + 'px'
    cursor.value.style.top = mouseY + 'px'
  }
}

// 鼠标点击事件
const handleMouseDown = () => {
  isClicking.value = true
}

const handleMouseUp = () => {
  isClicking.value = false
}

// 悬停事件
const handleMouseEnter = (e) => {
  if (e.target.matches('a, button, .clickable, [data-cursor="pointer"]')) {
    isHovering.value = true
  }
}

const handleMouseLeave = (e) => {
  if (e.target.matches('a, button, .clickable, [data-cursor="pointer"]')) {
    isHovering.value = false
  }
}

// 动画循环
const animate = () => {
  // 跟随光标平滑移动 - 提高跟随速度
  followerX += (mouseX - followerX) * 0.15
  followerY += (mouseY - followerY) * 0.15
  
  // 光环光标平滑移动 - 提高跟随速度
  ringX += (mouseX - ringX) * 0.08
  ringY += (mouseY - ringY) * 0.08
  
  // 更新跟随光标位置
  if (cursorFollower.value) {
    cursorFollower.value.style.left = followerX + 'px'
    cursorFollower.value.style.top = followerY + 'px'
  }
  
  // 更新光环位置
  if (cursorRing.value) {
    cursorRing.value.style.left = ringX + 'px'
    cursorRing.value.style.top = ringY + 'px'
  }
  
  animationId = requestAnimationFrame(animate)
}

// 添加可点击元素的属性
const addClickableAttributes = () => {
  const clickableElements = document.querySelectorAll('a, button, .clickable')
  clickableElements.forEach(el => {
    el.setAttribute('data-cursor', 'pointer')
  })
}

onMounted(() => {
  // 保持默认光标，不隐藏
  // document.body.style.cursor = 'none'
  
  // 添加事件监听器
  document.addEventListener('mousemove', handleMouseMove)
  document.addEventListener('mousedown', handleMouseDown)
  document.addEventListener('mouseup', handleMouseUp)
  document.addEventListener('mouseover', handleMouseEnter)
  document.addEventListener('mouseout', handleMouseLeave)
  
  // 启动动画
  animate()
  
  // 添加可点击元素属性
  setTimeout(addClickableAttributes, 100)
})

onUnmounted(() => {
  // 保持默认光标
  // document.body.style.cursor = 'auto'
  
  // 移除事件监听器
  document.removeEventListener('mousemove', handleMouseMove)
  document.removeEventListener('mousedown', handleMouseDown)
  document.removeEventListener('mouseup', handleMouseUp)
  document.removeEventListener('mouseover', handleMouseEnter)
  document.removeEventListener('mouseout', handleMouseLeave)
  
  // 停止动画
  if (animationId) {
    cancelAnimationFrame(animationId)
  }
})
</script>

<style scoped>
.dynamic-cursor {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 9999;
}

.cursor {
  position: absolute;
  width: 8px;
  height: 8px;
  background: linear-gradient(45deg, #ff6b6b, #4ecdc4, #45b7d1);
  border-radius: 50%;
  transform: translate(-50%, -50%);
  transition: all 0.1s ease;
  z-index: 10001;
  box-shadow: 0 0 15px rgba(255, 107, 107, 0.8), 0 0 30px rgba(78, 205, 196, 0.6);
  animation: cursorGlow 2s ease-in-out infinite alternate;
  display: none; /* 隐藏主光标，保持默认鼠标形状 */
}

.cursor-hover {
  width: 50px;
  height: 50px;
  background: linear-gradient(45deg, rgba(255, 107, 107, 0.2), rgba(78, 205, 196, 0.2));
  border: 2px solid #fff;
  backdrop-filter: blur(15px);
  box-shadow: 0 0 25px rgba(255, 255, 255, 0.5);
}

.cursor-click {
  width: 6px;
  height: 6px;
  background: #ff6b6b;
  box-shadow: 0 0 15px rgba(255, 107, 107, 0.8);
}

.cursor-follower {
  position: absolute;
  width: 25px;
  height: 25px;
  border: 2px solid rgba(255, 255, 255, 0.4);
  border-radius: 50%;
  transform: translate(-50%, -50%);
  transition: all 0.2s ease;
  z-index: 10000;
  background: radial-gradient(circle, rgba(255, 255, 255, 0.1) 0%, transparent 70%);
}

.follower-hover {
  width: 70px;
  height: 70px;
  border-color: rgba(255, 255, 255, 0.8);
  background: radial-gradient(circle, rgba(255, 255, 255, 0.15) 0%, transparent 70%);
  box-shadow: 0 0 20px rgba(255, 255, 255, 0.3);
}

.cursor-ring {
  position: absolute;
  width: 30px;
  height: 30px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 50%;
  transform: translate(-50%, -50%);
  transition: all 0.5s ease;
  z-index: 9999;
}

.ring-hover {
  width: 80px;
  height: 80px;
  border-color: rgba(255, 255, 255, 0.2);
  background: radial-gradient(circle, rgba(255, 255, 255, 0.1) 0%, transparent 70%);
}

/* 光标发光动画 */
@keyframes cursorGlow {
  0% {
    box-shadow: 0 0 15px rgba(255, 107, 107, 0.8), 0 0 30px rgba(78, 205, 196, 0.6);
  }
  100% {
    box-shadow: 0 0 25px rgba(255, 107, 107, 1), 0 0 50px rgba(78, 205, 196, 0.8);
  }
}

/* 移动端隐藏自定义光标 */
@media (max-width: 768px) {
  .dynamic-cursor {
    display: none;
  }
}

/* 为可点击元素添加悬停效果 */
:global([data-cursor="pointer"]:hover) {
  transform: scale(1.05);
  transition: transform 0.2s ease;
}

/* 按钮悬停效果 */
:global(button:hover) {
  transform: translateY(-2px);
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
  transition: all 0.3s ease;
}

/* 链接悬停效果 */
:global(a:hover) {
  text-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
  transition: text-shadow 0.3s ease;
}
</style>

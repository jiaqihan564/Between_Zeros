<template>
  <div class="bouncing-balls">
    <canvas ref="ballCanvas" class="ball-canvas"></canvas>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const ballCanvas = ref(null)
let animationId = null
let balls = []

// 彩虹圆圈类
class RainbowCircle {
  constructor(canvas) {
    this.canvas = canvas
    this.radius = Math.random() * 50 + 5 // 5-55px 半径，更大的变化范围
    this.x = Math.random() * (canvas.width - this.radius * 2) + this.radius
    this.y = Math.random() * (canvas.height - this.radius * 2) + this.radius
    this.vx = (Math.random() - 0.5) * 6 // -3 到 3 的速度，稍微慢一些
    this.vy = (Math.random() - 0.5) * 6
    this.colors = this.getRainbowColors()
    this.mass = this.radius * 0.1 // 质量与半径相关
    this.rotation = 0
    this.rotationSpeed = (Math.random() - 0.5) * 0.1 // 旋转速度
  }

  getRainbowColors() {
    // 创建彩虹渐变颜色数组
    const hue = Math.random() * 360 // 随机色相
    const colors = []
    for (let i = 0; i < 6; i++) {
      const currentHue = (hue + i * 60) % 360
      colors.push(`hsl(${currentHue}, 70%, 60%)`)
    }
    return colors
  }

  update() {
    // 更新位置
    this.x += this.vx
    this.y += this.vy
    
    // 更新旋转
    this.rotation += this.rotationSpeed

    // 边界碰撞检测
    if (this.x - this.radius <= 0 || this.x + this.radius >= this.canvas.width) {
      this.vx = -this.vx
      this.x = this.x - this.radius <= 0 ? this.radius : this.canvas.width - this.radius
    }
    if (this.y - this.radius <= 0 || this.y + this.radius >= this.canvas.height) {
      this.vy = -this.vy
      this.y = this.y - this.radius <= 0 ? this.radius : this.canvas.height - this.radius
    }
  }

  draw(ctx) {
    ctx.save()
    
    // 移动到圆心并旋转
    ctx.translate(this.x, this.y)
    ctx.rotate(this.rotation)
    
    // 绘制彩虹圆圈
    const segmentAngle = (Math.PI * 2) / this.colors.length
    
    for (let i = 0; i < this.colors.length; i++) {
      ctx.beginPath()
      ctx.moveTo(0, 0)
      ctx.arc(0, 0, this.radius, i * segmentAngle, (i + 1) * segmentAngle)
      ctx.closePath()
      
      // 创建径向渐变
      const gradient = ctx.createRadialGradient(0, 0, 0, 0, 0, this.radius)
      gradient.addColorStop(0, this.lightenColor(this.colors[i], 0.4))
      gradient.addColorStop(1, this.colors[i])
      
      ctx.fillStyle = gradient
      ctx.fill()
    }
    
    // 绘制外圈发光效果
    ctx.beginPath()
    ctx.arc(0, 0, this.radius, 0, Math.PI * 2)
    ctx.strokeStyle = 'rgba(255, 255, 255, 0.3)'
    ctx.lineWidth = 2
    ctx.stroke()
    
    // 添加整体发光阴影
    ctx.shadowColor = this.colors[0]
    ctx.shadowBlur = 15
    ctx.beginPath()
    ctx.arc(0, 0, this.radius * 0.8, 0, Math.PI * 2)
    ctx.fillStyle = 'rgba(255, 255, 255, 0.1)'
    ctx.fill()
    ctx.shadowBlur = 0
    
    ctx.restore()
  }

  lightenColor(color, amount) {
    // 解析HSL颜色
    const hslMatch = color.match(/hsl\((\d+),\s*(\d+)%,\s*(\d+)%\)/)
    if (hslMatch) {
      const h = parseInt(hslMatch[1])
      const s = parseInt(hslMatch[2])
      const l = Math.min(100, parseInt(hslMatch[3]) + amount * 100)
      return `hsl(${h}, ${s}%, ${l}%)`
    }
    return color
  }

  // 检查与其他球的碰撞
  checkCollision(other) {
    const dx = this.x - other.x
    const dy = this.y - other.y
    const distance = Math.sqrt(dx * dx + dy * dy)
    
    if (distance < this.radius + other.radius) {
      // 碰撞发生，计算碰撞响应
      this.resolveCollision(other)
      return true
    }
    return false
  }

  // 碰撞响应
  resolveCollision(other) {
    const dx = this.x - other.x
    const dy = this.y - other.y
    const distance = Math.sqrt(dx * dx + dy * dy)
    
    // 防止球体重叠
    const overlap = this.radius + other.radius - distance
    const separationX = (dx / distance) * overlap * 0.5
    const separationY = (dy / distance) * overlap * 0.5
    
    this.x += separationX
    this.y += separationY
    other.x -= separationX
    other.y -= separationY
    
    // 计算相对速度
    const relativeVx = this.vx - other.vx
    const relativeVy = this.vy - other.vy
    
    // 计算碰撞法向量
    const normalX = dx / distance
    const normalY = dy / distance
    
    // 计算相对速度在法向量上的投影
    const relativeSpeed = relativeVx * normalX + relativeVy * normalY
    
    // 如果球体正在分离，不需要处理碰撞
    if (relativeSpeed > 0) return
    
    // 计算碰撞冲量
    const impulse = 2 * relativeSpeed / (this.mass + other.mass)
    
    // 更新速度
    this.vx -= impulse * other.mass * normalX
    this.vy -= impulse * other.mass * normalY
    other.vx += impulse * this.mass * normalX
    other.vy += impulse * this.mass * normalY
  }
}

// 初始化彩虹圆圈
const initBalls = () => {
  const canvas = ballCanvas.value
  if (!canvas) return
  
  const ctx = canvas.getContext('2d')
  canvas.width = window.innerWidth
  canvas.height = window.innerHeight
  
  balls = []
  // 创建100个不同大小的彩虹圆圈
  for (let i = 0; i < 100; i++) {
    balls.push(new RainbowCircle(canvas))
  }
}

// 动画循环
const animate = () => {
  const canvas = ballCanvas.value
  if (!canvas) return
  
  const ctx = canvas.getContext('2d')
  ctx.clearRect(0, 0, canvas.width, canvas.height)
  
  // 更新所有圆圈
  balls.forEach(ball => {
    ball.update()
  })
  
  // 检查圆圈与圆圈之间的碰撞（优化：只检查附近的圆圈）
  for (let i = 0; i < balls.length; i++) {
    for (let j = i + 1; j < balls.length; j++) {
      const ball1 = balls[i]
      const ball2 = balls[j]
      
      // 距离检查优化：只检查可能碰撞的圆圈
      const dx = ball1.x - ball2.x
      const dy = ball1.y - ball2.y
      const distance = Math.sqrt(dx * dx + dy * dy)
      
      if (distance < ball1.radius + ball2.radius + 50) { // 50px的缓冲区域
        ball1.checkCollision(ball2)
      }
    }
  }
  
  // 绘制所有圆圈
  balls.forEach(ball => {
    ball.draw(ctx)
  })
  
  animationId = requestAnimationFrame(animate)
}

// 窗口大小改变事件
const handleResize = () => {
  const canvas = ballCanvas.value
  if (!canvas) return
  
  canvas.width = window.innerWidth
  canvas.height = window.innerHeight
  
  // 重新调整圆圈的位置，确保在画布内
  balls.forEach(ball => {
    if (ball.x + ball.radius > canvas.width) {
      ball.x = canvas.width - ball.radius
    }
    if (ball.y + ball.radius > canvas.height) {
      ball.y = canvas.height - ball.radius
    }
    if (ball.x - ball.radius < 0) {
      ball.x = ball.radius
    }
    if (ball.y - ball.radius < 0) {
      ball.y = ball.radius
    }
  })
}

onMounted(() => {
  initBalls()
  animate()
  window.addEventListener('resize', handleResize)
})

onUnmounted(() => {
  if (animationId) {
    cancelAnimationFrame(animationId)
  }
  window.removeEventListener('resize', handleResize)
})
</script>

<style scoped>
.bouncing-balls {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -2;
  pointer-events: none;
}

.ball-canvas {
  width: 100%;
  height: 100%;
}
</style>

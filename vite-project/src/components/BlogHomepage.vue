<template>
  <div class="blog-container">
    <!-- 头部导航 -->
    <header class="header">
      <nav class="nav">
        <div class="nav-brand">
          <h2>我的博客</h2>
        </div>
        <ul class="nav-links">
          <li><a href="#home" @click="scrollToSection('home', $event)">首页</a></li>
          <li><a href="#about" @click="scrollToSection('about', $event)">关于我</a></li>
          <li><a href="#projects" @click="scrollToSection('projects', $event)">项目</a></li>
          <li><a href="#contact" @click="scrollToSection('contact', $event)">联系</a></li>
        </ul>
      </nav>
    </header>

    <!-- 主要内容区域 -->
    <main class="main-content">
      <!-- 英雄区域 -->
      <section id="home" class="hero">
        <div class="hero-container">
          <div class="hero-content">
            <div class="hero-text">
              <h1 class="hero-title">
                你好，我是 <span class="highlight">Jspring</span>
              </h1>
              <p class="hero-subtitle">
                热爱编程，专注于后端开发，喜欢创造美好的用户体验
              </p>
              <div class="hero-buttons">
                <button class="btn btn-primary" @click="scrollToSection('projects')">查看我的作品</button>
                <button class="btn btn-secondary" @click="scrollToSection('contact')">联系我</button>
              </div>
            </div>
            <div class="hero-image">
              <div class="avatar-container">
                <div class="avatar">👨‍💻</div>
                <div class="avatar-glow"></div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- 关于我区域 -->
      <section id="about" class="about">
        <div class="section-container">
          <div class="section-header">
            <h2>关于我</h2>
            <p>了解我的技能和经历</p>
          </div>
          <div class="about-content">
            <div class="about-text">
              <p class="about-description">
                我是一名后端开发者，专注于后端开发，喜欢创造美好的用户体验。拥有丰富的项目经验，擅长构建高性能、可扩展的Web应用程序。
              </p>
              <div class="skills">
                <h3>技能栈</h3>
                <div class="skill-tags">
                  <span class="skill-tag">Java</span>
                  <span class="skill-tag">Spring</span>
                  <span class="skill-tag">Spring Boot</span>
                  <span class="skill-tag">MyBatis</span>
                  <span class="skill-tag">MySQL</span>
                  <span class="skill-tag">Redis</span>
                  <span class="skill-tag">Git</span>
                  <span class="skill-tag">Docker</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- 项目展示区域 -->
      <section id="projects" class="projects">
        <div class="section-container">
          <div class="section-header">
            <h2>精选项目</h2>
            <p>我的一些得意之作</p>
          </div>
        <div class="projects-grid">
          <div class="project-card">
            <div class="project-image">
              <div class="project-placeholder">📱</div>
            </div>
            <div class="project-content">
              <h3>个人博客系统</h3>
              <p>使用Vue 3 + Vite构建的现代化个人博客，支持响应式设计。</p>
              <div class="project-tech">
                <span class="tech-tag">Vue 3</span>
                <span class="tech-tag">Vite</span>
                <span class="tech-tag">CSS3</span>
              </div>
              <div class="project-links">
                <a href="#" class="project-link">查看项目</a>
                <a href="#" class="project-link">GitHub</a>
              </div>
            </div>
          </div>

          <div class="project-card">
            <div class="project-image">
              <div class="project-placeholder">🛒</div>
            </div>
            <div class="project-content">
              <h3>电商管理系统</h3>
              <p>全栈电商管理平台，包含商品管理、订单处理、用户管理等功能。</p>
              <div class="project-tech">
                <span class="tech-tag">React</span>
                <span class="tech-tag">Node.js</span>
                <span class="tech-tag">MongoDB</span>
              </div>
              <div class="project-links">
                <a href="#" class="project-link">查看项目</a>
                <a href="#" class="project-link">GitHub</a>
              </div>
            </div>
          </div>

          <div class="project-card">
            <div class="project-image">
              <div class="project-placeholder">📊</div>
            </div>
            <div class="project-content">
              <h3>数据可视化仪表板</h3>
              <p>实时数据展示平台，支持多种图表类型和交互式操作。</p>
              <div class="project-tech">
                <span class="tech-tag">D3.js</span>
                <span class="tech-tag">TypeScript</span>
                <span class="tech-tag">WebSocket</span>
              </div>
              <div class="project-links">
                <a href="#" class="project-link">查看项目</a>
                <a href="#" class="project-link">GitHub</a>
              </div>
            </div>
          </div>
        </div>
        </div>
      </section>

      <!-- 联系我区域 -->
      <section id="contact" class="contact">
        <div class="section-container">
          <div class="section-header">
            <h2>联系我</h2>
            <p>让我们一起创造美好的东西</p>
          </div>
        <div class="contact-content">
          <div class="contact-info">
            <div class="contact-item">
              <div class="contact-icon">📧</div>
              <div class="contact-details">
                <h4>邮箱</h4>
                <p>3259246363@qq.com</p>
              </div>
            </div>
            <div class="contact-item">
              <div class="contact-icon">💼</div>
              <div class="contact-details">
                <h4>LinkedIn</h4>
                <p>linkedin.com/in/yourprofile</p>
              </div>
            </div>
            <div class="contact-item">
              <div class="contact-icon">🐙</div>
              <div class="contact-details">
                <h4>GitHub</h4>
                <p>https://github.com/jiaqihan564/</p>
              </div>
            </div>
          </div>
          <div class="contact-form">
            <form>
              <div class="form-group">
                <input type="text" placeholder="你的姓名" required>
              </div>
              <div class="form-group">
                <input type="email" placeholder="你的邮箱" required>
              </div>
              <div class="form-group">
                <textarea placeholder="你的消息" rows="5" required></textarea>
              </div>
              <button type="submit" class="btn btn-primary">发送消息</button>
            </form>
          </div>
        </div>
        </div>
      </section>
    </main>

    <!-- 页脚 -->
    <footer class="footer">
      <div class="footer-content">
        <p>&copy; 2024 我的博客. 保留所有权利.</p>
        <div class="social-links">
          <a href="#" class="social-link">GitHub</a>
          <a href="#" class="social-link">LinkedIn</a>
          <a href="#" class="social-link">Twitter</a>
        </div>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// 平滑滚动到指定区域
const scrollToSection = (sectionId, event) => {
  if (event) {
    event.preventDefault()
  }
  const element = document.getElementById(sectionId)
  if (element) {
    element.scrollIntoView({
      behavior: 'smooth',
      block: 'start'
    })
  }
}
</script>

<style scoped>
/* 基础样式 */
.blog-container {
  min-height: 100vh;
  color: #333;
}

/* 通用容器样式 */
.section-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
}

/* 头部导航 */
.header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  z-index: 1000;
  box-shadow: 0 2px 20px rgba(0, 0, 0, 0.1);
}

.nav {
  max-width: 1200px;
  margin: 0 auto;
  padding: 1rem 2rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.nav-brand h2 {
  margin: 0;
  color: #667eea;
  font-weight: 700;
}

.nav-links {
  display: flex;
  list-style: none;
  margin: 0;
  padding: 0;
  gap: 2rem;
}

.nav-links a {
  text-decoration: none;
  color: #333;
  font-weight: 500;
  transition: color 0.3s ease;
}

.nav-links a:hover {
  color: #667eea;
}

/* 主要内容 */
.main-content {
  padding-top: 80px;
}

/* 英雄区域 */
.hero {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 6rem 0 4rem;
  color: white;
}

.hero-container {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
}

.hero-content {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 4rem;
  align-items: center;
}

.hero-text {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.hero-title {
  font-size: 3.5rem;
  font-weight: 700;
  line-height: 1.2;
  margin-bottom: 1.5rem;
}

.highlight {
  background: linear-gradient(45deg, #ffd700, #ffed4e);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.hero-subtitle {
  font-size: 1.25rem;
  margin-bottom: 2rem;
  opacity: 0.9;
  line-height: 1.6;
}

.hero-buttons {
  display: flex;
  gap: 1rem;
}

.btn {
  padding: 0.75rem 2rem;
  border: none;
  border-radius: 50px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  text-decoration: none;
  display: inline-block;
  text-align: center;
}

.btn-primary {
  background: linear-gradient(45deg, #ffd700, #ffed4e);
  color: #333;
}

.btn-primary:hover {
  transform: translateY(-2px);
  box-shadow: 0 10px 25px rgba(255, 215, 0, 0.3);
}

.btn-secondary {
  background: transparent;
  color: white;
  border: 2px solid white;
}

.btn-secondary:hover {
  background: white;
  color: #333;
}

.hero-image {
  display: flex;
  justify-content: center;
  align-items: center;
}

.avatar-container {
  position: relative;
  width: 300px;
  height: 300px;
}

.avatar {
  width: 100%;
  height: 100%;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 6rem;
  border: 2px solid rgba(255, 255, 255, 0.2);
  position: relative;
  z-index: 2;
}

.avatar-glow {
  position: absolute;
  top: -20px;
  left: -20px;
  right: -20px;
  bottom: -20px;
  background: radial-gradient(circle, rgba(255, 255, 255, 0.1) 0%, transparent 70%);
  border-radius: 50%;
  animation: pulse 2s ease-in-out infinite alternate;
}

@keyframes pulse {
  0% { transform: scale(1); opacity: 0.5; }
  100% { transform: scale(1.1); opacity: 0.8; }
}

/* 通用区域样式 */
section {
  padding: 4rem 0;
}

.section-header {
  text-align: center;
  margin-bottom: 3rem;
}

.section-header h2 {
  font-size: 2.5rem;
  font-weight: 700;
  margin-bottom: 1rem;
  color: white;
}

.section-header p {
  font-size: 1.2rem;
  opacity: 0.9;
  color: white;
}

/* 关于我区域 */
.about {
  color: white;
}

.about-description {
  font-size: 1.2rem;
  line-height: 1.8;
  margin-bottom: 2rem;
  opacity: 0.9;
}

.section-header p {
  font-size: 1.1rem;
  color: rgba(255, 255, 255, 0.8);
}

/* 关于我区域 */
.about {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  margin: 2rem;
}

.about-text p {
  font-size: 1.1rem;
  line-height: 1.8;
  color: white;
  margin-bottom: 2rem;
}

.skills h3 {
  color: white;
  margin-bottom: 1rem;
}

.skill-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.skill-tag {
  background: rgba(255, 255, 255, 0.3);
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 25px;
  font-size: 0.9rem;
  text-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
}

/* 项目展示区域 */
.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
  gap: 2rem;
}

.project-card {
  background: rgba(255, 255, 255, 0.2);
  border-radius: 15px;
  overflow: hidden;
  transition: transform 0.3s ease;
  text-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
}

.project-card:hover {
  transform: translateY(-5px);
}

.project-image {
  height: 200px;
  background: rgba(255, 255, 255, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
}

.project-placeholder {
  font-size: 4rem;
}

.project-content {
  padding: 1.5rem;
  color: white;
}

.project-content h3 {
  margin-bottom: 1rem;
  color: white;
}

.project-content p {
  margin-bottom: 1rem;
  opacity: 0.9;
  line-height: 1.6;
}

.project-tech {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-bottom: 1rem;
}

.tech-tag {
  background: rgba(255, 255, 255, 0.3);
  color: white;
  padding: 0.25rem 0.75rem;
  border-radius: 15px;
  font-size: 0.8rem;
  text-shadow: 0 0 5px rgba(0, 0, 0, 0.5);
}

.project-links {
  display: flex;
  gap: 1rem;
}

.project-link {
  color: #ffd700;
  text-decoration: none;
  font-weight: 600;
  transition: color 0.3s ease;
}

.project-link:hover {
  color: #ffed4e;
}

/* 联系我区域 */
.contact {
  background: rgba(255, 255, 255, 0.2);
  border-radius: 20px;
  margin: 2rem;
  text-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
}

.contact-content {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 3rem;
}

.contact-item {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-bottom: 2rem;
  color: white;
}

.contact-icon {
  font-size: 2rem;
  width: 60px;
  height: 60px;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.contact-details h4 {
  margin: 0 0 0.5rem 0;
  color: white;
}

.contact-details p {
  margin: 0;
  opacity: 0.8;
}

.contact-form {
  color: white;
}

.form-group {
  margin-bottom: 1.5rem;
}

.form-group input,
.form-group textarea {
  width: 100%;
  padding: 1rem;
  border: none;
  border-radius: 10px;
  background: rgba(255, 255, 255, 0.1);
  color: white;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.form-group input::placeholder,
.form-group textarea::placeholder {
  color: rgba(255, 255, 255, 0.7);
}

.form-group input:focus,
.form-group textarea:focus {
  outline: none;
  border-color: #ffd700;
}

/* 页脚 */
.footer {
  background: rgba(0, 0, 0, 0.2);
  color: white;
  text-align: center;
  padding: 2rem;
}

.footer-content {
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.social-links {
  display: flex;
  gap: 1rem;
}

.social-link {
  color: white;
  text-decoration: none;
  transition: color 0.3s ease;
}

.social-link:hover {
  color: #ffd700;
}

/* 响应式设计 */
@media (max-width: 768px) {
  .section-container {
    padding: 0 1rem;
  }
  
  .nav {
    flex-direction: column;
    gap: 1rem;
  }
  
  .nav-links {
    gap: 1rem;
  }
  
  .hero {
    padding: 4rem 0 2rem;
  }
  
  .hero-content {
    grid-template-columns: 1fr;
    text-align: center;
    gap: 2rem;
  }
  
  .hero-title {
    font-size: 2.5rem;
  }
  
  .hero-subtitle {
    font-size: 1.1rem;
  }
  
  .avatar-container {
    width: 250px;
    height: 250px;
  }
  
  .avatar {
    font-size: 5rem;
  }
  
  .section-header h2 {
    font-size: 2rem;
  }
  
  .hero-buttons {
    justify-content: center;
    flex-direction: column;
    gap: 1rem;
  }
  
  .btn {
    width: 100%;
  }
  
  .contact-content {
    grid-template-columns: 1fr;
    gap: 2rem;
  }
  
  .footer-content {
    flex-direction: column;
    gap: 1rem;
  }
  
  .projects-grid {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 480px) {
  .hero-title {
    font-size: 2rem;
  }
  
  .section-header h2 {
    font-size: 2rem;
  }
  
  .hero-buttons {
    flex-direction: column;
    align-items: center;
  }
}
</style>

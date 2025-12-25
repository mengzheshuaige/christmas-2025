<script setup>
  import { ref } from 'vue'
  import Gallery from './components/Gallery.vue'
  import MusicPlayer from './components/MusicPlayer.vue'
  
  const isStarted = ref(false)
  const musicRef = ref(null)
  
  const startExperience = () => {
    isStarted.value = true
    // 延迟播放音乐，配合入场动画的呼吸感
    setTimeout(() => musicRef.value?.play(), 200)
  }
  </script>
  
  <template>
    <div class="noel-app">
      <Transition name="entry-blur">
        <div v-if="!isStarted" class="intro-overlay" @click="startExperience">
          <div class="glass-seal">
            <div class="gift-trigger">
              <div class="sparkle">✦</div>
              <div class="text-group">
                <p class="en-label">UNWRAP THE MAGIC</p>
                <div class="line"></div>
                <p class="special-to">To: 陆敏蔚</p>
              </div>
              <div class="sparkle-bottom">✦</div>
            </div>
          </div>
          <div class="bg-glow"></div>
        </div>
      </Transition>
  
      <main v-if="isStarted" class="main-stage">
        <div class="ambient-gradient"></div>
        <div class="vignette"></div>
        
        <header class="luxury-header">
          <div class="title-container">
            <h1 data-text="聖誕節快樂">聖誕節快乐</h1>
            <div class="title-glow"></div>
          </div>
          <p class="subtitle">EST. 2025 / 🧯🧯🧯</p>
        </header>
  
        <section class="gallery-container">
          <Gallery />
        </section>
        
        <MusicPlayer ref="musicRef" />
      </main>
  
      <div class="snow-field">
        <div v-for="n in 35" :key="n" class="flake"></div>
      </div>
    </div>
  </template>
  
  <style>
  /* 引入顶级奢侈品感字体 */
  @import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@400;700&family=Noto+Serif+SC:wght@900&display=swap');
  
  :root {
    --gold: #d4af37;
    --gold-light: #fcf6ba;
    --bg-deep: #020808;
    --emerald-glow: rgba(2, 43, 29, 0.4);
  }
  
  * { -webkit-tap-highlight-color: transparent; outline: none; }
  
  body, html { 
    margin: 0; 
    padding: 0; 
    background: var(--bg-deep); 
    color: #fff; 
    overflow: hidden;
    font-family: 'Cinzel', serif;
    width: 100%;
    height: 100%;
  }
  
  /* --- 入场遮罩设计 --- */
  .intro-overlay {
    position: fixed;
    inset: 0;
    z-index: 1000;
    background: radial-gradient(circle at center, #0a0a0a 0%, #000 100%);
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    overflow: hidden;
  }
  
  .glass-seal {
    position: relative;
    padding: 40px;
    background: rgba(255, 255, 255, 0.03);
    backdrop-filter: blur(10px);
    border: 1px solid rgba(212, 175, 55, 0.2);
    border-radius: 2px;
    box-shadow: 0 0 50px rgba(0,0,0,0.5);
    z-index: 2;
  }
  
  .gift-trigger { text-align: center; color: var(--gold); }
  
  .sparkle, .sparkle-bottom { font-size: 1.5rem; opacity: 0.6; animation: rotateStar 4s infinite linear; }
  
  .text-group { margin: 25px 0; }
  
  .en-label { font-size: 0.7rem; letter-spacing: 0.6rem; opacity: 0.8; margin-bottom: 10px; }
  
  .special-to {
    font-family: 'Noto Serif SC', serif;
    font-size: 1.2rem;
    letter-spacing: 0.3rem;
    margin-top: 15px;
    color: #fff;
  }
  
  .gift-trigger .line {
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--gold), transparent);
    width: 150px;
    margin: 0 auto;
    transition: width 1s cubic-bezier(0.4, 0, 0.2, 1);
  }
  
  .intro-overlay:hover .line { width: 220px; }
  
  明白了，这种“距离感”通常是因为 flex: 1 占据了屏幕所有剩余空间并把照片强行居中导致的。

为了让标题和照片像精心排版的杂志一样紧凑、成对出现，我们需要彻底抛弃“自动平分空间”的逻辑，改用精准的间距控制。

以下是优化后的 App.vue 样式，它将距离锁死，无论屏幕多大，照片都会紧跟在标题下方：

1. 更新 src/App.vue 的样式
请替换 <style> 部分中的相关类，或者直接覆盖：

CSS

/* --- 主场景布局：由分散改为聚合 --- */
.main-stage {
  display: flex;
  flex-direction: column;
  align-items: center;
  min-height: 100vh; 
  width: 100vw;
  position: relative;
  overflow-y: auto;
  overflow-x: hidden;
}
  
  .ambient-gradient {
    position: absolute;
    inset: 0;
    background: radial-gradient(circle at 50% 40%, var(--emerald-glow) 0%, transparent 70%);
    z-index: 0;
  }
  
  .luxury-header {
  /* 缩小顶部留白，让标题上移 */
  padding-top: 5vh; 
  padding-bottom: 0;
  width: 100%;
  text-align: center;
  z-index: 10;
}
.gallery-container {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: flex-start; /* 靠顶对齐 */
  /* 这里控制标题和照片的死距离：40px */
  margin-top: 20px; 
  padding-bottom: 40px; /* 给底部留一点呼吸空间 */
  z-index: 5;
}
  
  /* --- 核心标题动画 --- */
  .title-container { position: relative; display: inline-block; }
  
  h1 {
    font-family: 'Noto Serif SC', serif;
    font-size: clamp(2.5rem, 10vw, 4.5rem);
    margin: 0;
    letter-spacing: 0.8rem;
    position: relative;
    /* 制作流光金效果 */
    background: linear-gradient(
      90deg, 
      #fff 0%, 
      var(--gold) 25%, 
      var(--gold-light) 50%, 
      var(--gold) 75%, 
      #fff 100%
    );
    background-size: 200% auto;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    animation: shineText 5s linear infinite;
  }
  
  .title-glow {
    position: absolute;
    top: 50%; left: 50%;
    transform: translate(-50%, -50%);
    width: 120%; height: 60px;
    background: var(--gold);
    filter: blur(60px);
    opacity: 0.15;
    z-index: -1;
  }
  
  .subtitle {
    font-size: 0.7rem;
    letter-spacing: 0.4rem;
    margin-top: 15px;
    opacity: 0.5;
    font-weight: 200;
  }
  
  /* --- 动画 --- */
  @keyframes shineText { to { background-position: 200% center; } }
  @keyframes rotateStar { to { transform: rotate(360deg); } }
  
  .entry-blur-leave-active { transition: all 1.5s cubic-bezier(0.7, 0, 0.3, 1); }
  .entry-blur-leave-to { opacity: 0; filter: blur(30px) scale(1.1); }
  
  /* --- 飘雪优化 --- */
  .snow-field { position: fixed; inset: 0; pointer-events: none; z-index: 10; }
  .flake {
    position: absolute;
    top: -10px;
    background: #fff;
    border-radius: 50%;
    opacity: 0.4;
    animation: fall linear infinite;
  }
  
  @keyframes fall {
    to { transform: translateY(105vh) translateX(50px); }
  }
  
  /* 随机雪花分布 */
  .flake:nth-child(even) { width: 3px; height: 3px; }
  .flake:nth-child(odd) { width: 1px; height: 1px; }
  
  /* 响应式调整 */
  @media (max-width: 768px) {
  .luxury-header {
    padding-top: 40px; /* 手机端标题离顶 40px */
  }
  h1 {
    font-size: 2.2rem; /* 减小字号，避免占位过多 */
    letter-spacing: 0.3rem;
  }
  .gallery-container {
    margin-top: 10px; /* 手机端距离更近 */
  }
}

/* 针对超大屏电脑：限制照片高度，不让它把页面撑得太离谱 */
@media (min-width: 1200px) {
  .gallery-container {
    margin-top: 30px;
  }
}
  </style>
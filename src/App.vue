<script setup>
  import { ref } from 'vue'
  import Gallery from './components/Gallery.vue'
  import MusicPlayer from './components/MusicPlayer.vue'
  
  const isStarted = ref(false)
  const musicRef = ref(null)
  
  const startExperience = () => {
    isStarted.value = true
    setTimeout(() => musicRef.value?.play(), 200)
  }
  </script>
  
  <template>
    <div class="noel-app">
      <Transition name="entry-blur">
        <div v-if="!isStarted" class="intro-overlay" @click="startExperience">
          <div class="gift-trigger">
            <div class="sparkle">✦</div>
            <p>UNWRAP THE MAGIC</p>
            <p class="special-to">To: 陆敏蔚</p>
            <div class="line"></div>
          </div>
        </div>
      </Transition>
  
      <main v-if="isStarted" class="main-stage">
        <div class="vignette"></div>
        
        <header class="luxury-title">
          <h1 data-text="聖誕節快樂">聖誕節快乐</h1>
          <p class="subtitle">EST. 2025 / 🧯🧯🧯</p>
        </header>
  
        <Gallery />
        
        <MusicPlayer ref="musicRef" />
      </main>
  
      <div class="snow-field">
        <div v-for="n in 30" :key="n" class="flake"></div>
      </div>
    </div>
  </template>
  
  <style>
  @import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@700&family=Noto+Serif+SC:wght@900&display=swap');
  
  :root {
    --gold: #d4af37;
    --bg: #050a0a;
  }
  
  body, html { margin: 0; padding: 0; background: var(--bg); color: #fff; overflow: hidden; }
  
  /* 进场遮罩 */
  .intro-overlay {
    position: fixed;
    inset: 0;
    z-index: 1000;
    background: #000;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
  }
  
  .gift-trigger {
    text-align: center;
    color: var(--gold);
    letter-spacing: 0.5rem;
    font-family: 'Cinzel', serif;
  }
  
  .gift-trigger .line {
    height: 1px;
    background: var(--gold);
    width: 0;
    margin: 10px auto;
    transition: 0.8s ease;
  }
  
  .gift-trigger:hover .line { width: 100%; }
  
  /* 标题设计 */
  .luxury-title {
    position: absolute;
    top: 8vh;
    left: 50%;
    transform: translateX(-50%);
    text-align: center;
    z-index: 5;
  }
  
  .chinese-title, h1 {
    font-family: 'Noto Serif SC', serif;
    font-size: 4rem;
    margin: 0;
    letter-spacing: 1rem;
    background: linear-gradient(180deg, #fff 0%, var(--gold) 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    filter: drop-shadow(0 0 20px rgba(212, 175, 55, 0.3));
  }
  
  .subtitle {
    font-family: 'Cinzel', serif;
    font-size: 0.8rem;
    letter-spacing: 0.3rem;
    margin-top: 10px;
    opacity: 0.6;
  }
  
  /* 进场动画 */
  .entry-blur-leave-active { transition: all 1.2s cubic-bezier(0.8, 0, 0.2, 1); }
  .entry-blur-leave-to { opacity: 0; filter: blur(50px) scale(1.2); }
  
  /* 背景环境 */
  .vignette {
    position: fixed;
    inset: 0;
    background: radial-gradient(circle, transparent 20%, rgba(0,0,0,0.8) 100%);
    pointer-events: none;
  }
  </style>
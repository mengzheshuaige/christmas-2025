<template>
  <div class="gallery-container">
    <div class="photo-perspective">
      <Transition name="photo-flip" mode="out-in">
        <div :key="currentIndex" class="photo-frame" @click="handleAction">
          <div class="shimmer"></div>
          <img :src="photoList[currentIndex]" class="content-img" />
          <div class="frame-border"></div>
        </div>
      </Transition>
    </div>

    <Teleport to="body">
      <div v-for="toy in toys" :key="toy.id" class="toy-particle" :style="toy.style">
        {{ toy.icon }}
      </div>
    </Teleport>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const photoList = ref([



  '/images/1.jpg',



 '/images/11.jpg','/images/12.jpg','/images/13.jpg','/images/14.jpg','/images/15.jpg','/images/16.jpg','/images/17.jpg','/images/18.jpg','/images/19.jpg',



])

const currentIndex = ref(0)
const toys = ref([])
const icons = ['🎁', '🦌', '🔔', '🎄', '❄️', '✨', '🎅', '🦯']

const handleAction = (e) => {
  // 爆炸式掉落
  for (let i = 0; i < 8; i++) {
    const id = Math.random()
    const angle = Math.random() * Math.PI * 2
    const distance = 100 + Math.random() * 200
    
    toys.value.push({
      id,
      icon: icons[Math.floor(Math.random() * icons.length)],
      style: {
        left: e.clientX + 'px',
        top: e.clientY + 'px',
        '--tx': Math.cos(angle) * distance + 'px',
        '--ty': window.innerHeight + 'px',
        fontSize: (1.5 + Math.random() * 2) + 'rem'
      }
    })
    setTimeout(() => { toys.value = toys.value.filter(t => t.id !== id) }, 3000)
  }

  // 40% 概率切图
  if (Math.random() < 0.4) {
    currentIndex.value = (currentIndex.value + 1) % photoList.value.length
  }
}
</script>

<style scoped>
.gallery-container { height: 100vh; display: flex; align-items: center; justify-content: center; padding-top: 10vh; }
.photo-perspective { perspective: 2000px; }

.photo-frame {
  position: relative;
  width: 70vw; max-width: 380px; aspect-ratio: 3/4;
  background: #111;
  border: 1px solid rgba(212, 175, 55, 0.2);
  transform-style: preserve-3d;
  transition: transform 0.3s;
  cursor: pointer;
  box-shadow: 0 40px 100px rgba(0,0,0,0.8);
}

.photo-frame:active { transform: translateZ(-50px) rotateX(5deg); }

.content-img { width: 100%; height: 100%; object-fit: cover; filter: brightness(0.9) contrast(1.1); }

.shimmer {
  position: absolute; inset: 0; z-index: 2;
  background: linear-gradient(105deg, transparent 40%, rgba(255,255,255,0.1) 50%, transparent 60%);
  background-size: 200% 100%;
  animation: shine 4s infinite;
}

/* 爆炸掉落动画 */
.toy-particle {
  position: fixed; z-index: 1000; pointer-events: none;
  animation: explodeAndFall 2.5s cubic-bezier(0.1, 0.8, 0.3, 1) forwards;
}

@keyframes explodeAndFall {
  0% { transform: translate(-50%, -50%) scale(0); opacity: 1; }
  20% { transform: translate(calc(-50% + var(--tx)), -100px) scale(1.2) rotate(180deg); }
  100% { transform: translate(calc(-50% + var(--tx)), var(--ty)) scale(0.8) rotate(720deg); opacity: 0; }
}

@keyframes shine { 0% { background-position: 200% 0; } 100% { background-position: -200% 0; } }

/* 切图过渡：像翻开沉重的画册 */
.photo-flip-enter-active, .photo-flip-leave-active { transition: all 0.7s cubic-bezier(0.6, 0, 0.4, 1); }
.photo-flip-enter-from { opacity: 0; transform: rotateY(-90deg) scale(0.8); }
.photo-flip-leave-to { opacity: 0; transform: rotateY(90deg) scale(1.2); }
</style>
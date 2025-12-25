<template>
  <div class="gallery-wrapper">
    <div class="stage-3d">
      <Transition name="photo-zoom" mode="out-in">
        <div 
          :key="currentIndex" 
          class="photo-card"
          @click="handleClick"
        >
          <div class="glass-reflection"></div>
          <img :src="photoList[currentIndex]" alt="Gallery" />
        </div>
      </Transition>
    </div>

    <Teleport to="body">
      <div v-for="toy in toys" :key="toy.id" class="falling-toy" :style="toy.style">
        {{ toy.icon }}
      </div>
    </Teleport>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// 导入你的照片
const photoList = ref([
  '/images/1.jpg',
 '/images/11.jpg','/images/12.jpg','/images/13.jpg','/images/14.jpg','/images/15.jpg','/images/16.jpg','/images/17.jpg','/images/18.jpg','/images/19.jpg',
])

const currentIndex = ref(0)
const toys = ref([])
const toyIcons = ['🎅', '🎄', '🎁', '🦌', '🔔', '✨', '🧣', '🍬']

const handleClick = (e) => {
  // 1. 生成掉落物
  const count = 5 + Math.floor(Math.random() * 5)
  for (let i = 0; i < count; i++) {
    const id = Date.now() + Math.random()
    toys.value.push({
      id,
      icon: toyIcons[Math.floor(Math.random() * toyIcons.length)],
      style: {
        left: Math.random() * 100 + 'vw',
        fontSize: (1.5 + Math.random() * 2) + 'rem',
        animationDelay: (Math.random() * 0.5) + 's',
        animationDuration: (2 + Math.random() * 2) + 's'
      }
    })
    // 自动清理
    setTimeout(() => {
      toys.value = toys.value.filter(t => t.id !== id)
    }, 4000)
  }

  // 2. 概率切换照片
  if (Math.random() > 0.4) {
    currentIndex.value = (currentIndex.value + 1) % photoList.value.length
  }
}
</script>

<style scoped>
.gallery-wrapper {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.stage-3d {
  perspective: 1500px;
}

.photo-card {
  position: relative;
  width: 40vh;
  aspect-ratio: 3 / 4;
  border: 1px solid rgba(255,255,255,0.1);
  background: #111;
  padding: 10px;
  cursor: pointer;
  box-shadow: 0 30px 60px rgba(0,0,0,0.8);
  transition: transform 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.photo-card:hover {
  transform: rotateX(5deg) rotateY(5deg) scale(1.02);
}

.photo-card img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  filter: contrast(1.1);
}

.glass-reflection {
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg, rgba(255,255,255,0.1) 0%, transparent 50%);
  pointer-events: none;
  z-index: 2;
}

/* 掉落物动画 */
.falling-toy {
  position: fixed;
  top: -10vh;
  z-index: 100;
  pointer-events: none;
  animation: dropRotate linear forwards;
}

@keyframes dropRotate {
  0% { transform: translateY(0) rotate(0deg); opacity: 0; }
  10% { opacity: 1; }
  90% { opacity: 1; }
  100% { transform: translateY(110vh) rotate(720deg); opacity: 0; }
}

/* 翻页动画 */
.photo-zoom-enter-active, .photo-zoom-leave-active {
  transition: all 0.8s cubic-bezier(0.4, 0, 0.2, 1);
}
.photo-zoom-enter-from { opacity: 0; transform: scale(0.8) translateZ(-500px); }
.photo-zoom-leave-to { opacity: 0; transform: scale(1.2) translateZ(500px); }
</style>
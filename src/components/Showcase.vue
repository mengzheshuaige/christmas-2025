<template>
    <div class="showcase-area">
      <Transition name="photo-swap" mode="out-in">
        <div 
          :key="currentIndex" 
          class="photo-frame"
          @click="handleInteraction"
        >
          <img :src="photos[currentIndex]" class="main-photo" />
          <div class="frame-edge"></div>
        </div>
      </Transition>
  
      <Teleport to="body">
        <div 
          v-for="item in ornaments" 
          :key="item.id" 
          class="ornament"
          :style="{ left: item.x + 'vw', fontSize: item.size + 'rem' }"
        >
          {{ item.icon }}
        </div>
      </Teleport>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue'
  
  const photos = ref([
    'https://picsum.photos/800/1000?random=201',
    'https://picsum.photos/800/1000?random=202',
    'https://picsum.photos/800/1000?random=203',
    'https://picsum.photos/800/1000?random=204'
  ])
  
  const currentIndex = ref(0)
  const ornaments = ref([])
  const icons = ['🔔', '🎄', '🎁', '🦌', '🕯️', '🍪', '🦯', '🧦']
  
  const handleInteraction = (e) => {
    // 1. 触发掉落逻辑 (一次掉落3-5个)
    const burstCount = 3 + Math.floor(Math.random() * 3)
    for(let i=0; i<burstCount; i++) {
      const id = Math.random()
      ornaments.value.push({
        id,
        x: Math.random() * 90 + 5, // 随机水平位置
        size: Math.random() * 2 + 1.5,
        icon: icons[Math.floor(Math.random() * icons.length)]
      })
      setTimeout(() => {
        ornaments.value = ornaments.value.filter(o => o.id !== id)
      }, 4000)
    }
  
    // 2. 概率切换照片 (30% 概率)
    if (Math.random() < 0.3) {
      currentIndex.value = (currentIndex.value + 1) % photos.value.length
    }
  }
  </script>
  
  <style scoped>
  .showcase-area {
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    perspective: 2000px;
  }
  
  .photo-frame {
    position: relative;
    width: 45vh;
    aspect-ratio: 3/4;
    cursor: pointer;
    box-shadow: 0 50px 100px rgba(0,0,0,0.8);
    border: 1px solid rgba(197, 160, 89, 0.3);
    background: #111;
  }
  
  .main-photo {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
  }
  
  .frame-edge {
    position: absolute;
    inset: -15px;
    border: 1px solid var(--gold);
    pointer-events: none;
    opacity: 0.3;
  }
  
  /* 掉落物动画 */
  .ornament {
    position: fixed;
    top: -100px;
    z-index: 100;
    pointer-events: none;
    animation: fallRotate 4s cubic-bezier(0.25, 0.46, 0.45, 0.94) forwards;
  }
  
  @keyframes fallRotate {
    0% { transform: translateY(0) rotate(0deg); opacity: 0; }
    10% { opacity: 1; }
    100% { transform: translateY(110vh) rotate(720deg); opacity: 0.5; }
  }
  
  /* 照片切换动画 */
  .photo-swap-enter-active, .photo-swap-leave-active {
    transition: all 0.8s cubic-bezier(0.645, 0.045, 0.355, 1);
  }
  .photo-swap-enter-from { transform: translateX(100px) rotateY(-20deg); opacity: 0; }
  .photo-swap-leave-to { transform: translateX(-100px) rotateY(20deg); opacity: 0; }
  </style>
<template>
  <div class="audio-widget">
    <audio ref="audioPlayer" loop src="/jingle-bells.mp3"></audio>
    <div class="bars" :class="{ active: isPlaying }" @click="toggle">
      <div v-for="i in 4" :key="i" class="bar"></div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const isPlaying = ref(false)
const audioPlayer = ref(null)

const play = () => {
  audioPlayer.value.play()
  isPlaying.value = true
}

const toggle = () => {
  if (isPlaying.value) audioPlayer.value.pause()
  else audioPlayer.value.play()
  isPlaying.value = !isPlaying.value
}

defineExpose({ play })
</script>

<style scoped>
.audio-widget {
  position: fixed;
  bottom: 50px;
  right: 50px;
  cursor: pointer;
}

.bars {
  display: flex;
  align-items: flex-end;
  gap: 4px;
  height: 20px;
}

.bar {
  width: 2px;
  height: 5px;
  background: #d4af37;
  transition: 0.3s;
}

.bars.active .bar {
  animation: wave 0.6s infinite alternate;
}

.bar:nth-child(2) { animation-delay: 0.1s; }
.bar:nth-child(3) { animation-delay: 0.2s; }
.bar:nth-child(4) { animation-delay: 0.3s; }

@keyframes wave {
  from { height: 5px; }
  to { height: 20px; }
}
</style>
<template>
  <div class="minimal-audio">
    <audio ref="player" loop src="/jingle-bells.mp3"></audio>
    <div class="audio-bars" @click="toggle" :class="{ active: isPlaying }">
      <span v-for="i in 5" :key="i"></span>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const isPlaying = ref(false)
const player = ref(null)
const play = () => { player.value.play(); isPlaying.value = true; }
const toggle = () => {
  if (isPlaying.value) player.value.pause()
  else player.value.play()
  isPlaying.value = !isPlaying.value
}
defineExpose({ play })
</script>

<style scoped>
.minimal-audio { position: fixed; bottom: 40px; right: 40px; z-index: 500; }
.audio-bars { display: flex; align-items: flex-end; gap: 3px; height: 25px; cursor: pointer; padding: 10px; }
.audio-bars span { width: 2px; height: 5px; background: #d4af37; transition: 0.2s; }
.audio-bars.active span { animation: barFlow 0.6s infinite alternate; }
.audio-bars span:nth-child(2) { animation-delay: 0.1s; }
.audio-bars span:nth-child(3) { animation-delay: 0.2s; }
.audio-bars span:nth-child(4) { animation-delay: 0.3s; }
.audio-bars span:nth-child(5) { animation-delay: 0.4s; }
@keyframes barFlow { from { height: 5px; } to { height: 25px; } }
</style>
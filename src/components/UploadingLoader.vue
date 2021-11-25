<template>
  <div>
    <h1>Uploading...</h1>
    <div class="progress-wrapper">
      <div class="progress-fill" />
    </div>
  </div>
</template>

<script lang="ts" setup>
const emit = defineEmits<{
  (event: 'uploaded'): void
}>();

setTimeout(() => {
  console.log('emit')
  emit('uploaded')
}, 1800);
</script>

<style scoped>
@keyframes animate-progress {
  from {
    transform: translateX(-150px);
  }

  to {
    transform: translateX(368px);
  }
}

h1 {
  text-align: left;
  margin-bottom: 1rem;
}

.progress-wrapper {
  width: 100%;
  height: 6px;
  background: #f2f2f2;
  border-radius: 8px;
  overflow: hidden;
}

.progress-fill {
  background-color: #2f80ed;
  width: 150px;
  height: 100%;
  border-radius: 10px;
  animation-name: animate-progress;
  animation-duration: 2s;
  animation-iteration-count: infinite;
  animation-timing-function: ease-in-out;
}
</style>
<template>
  <div id="widget-wrapper" :style="wrapperStyle">
    <iframe :src="iframeSrc" :style="iframeStyle" @load="onLoad" />
    <button @click="toggleWidget" class="widget-toggle-btn">Chat</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const iframeVisible = ref(false)
const iframeSrc = "https://cdn.talkjs.com/widget-frame.html"

const wrapperStyle = {
  position: 'fixed',
  right: '20px',
  bottom: '20px',
  zIndex: '9999',
}

const iframeStyle = {
  width: '350px',
  height: '500px',
  display: iframeVisible.value ? 'block' : 'none',
  border: 'none',
}

function toggleWidget() {
  iframeVisible.value = !iframeVisible.value
}

function onLoad() {
  window.addEventListener("message", (event) => {
    if (event.origin !== 'https://cdn.talkjs.com') return;
    // Handle messages like 'close', 'open', etc.
  })
}
</script>

<style scoped>
.widget-toggle-btn {
  position: absolute;
  bottom: 0;
  right: 0;
  padding: 10px;
  background: #0084ff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
</style>

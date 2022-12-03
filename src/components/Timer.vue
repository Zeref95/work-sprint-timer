<script setup lang="ts">
import {computed, createApp, ref} from 'vue'

const timerIsRunning = ref(false);
const timeLeft = ref(0);
let interval: ReturnType<typeof setInterval> = 0;

const startTimer = (minutes: number) => {
  timeLeft.value = minutes * 60;
  timerIsRunning.value = true;
  interval = setInterval(() => {
    timeLeft.value--;
    if (timeLeft.value === 0) {
      clearInterval(interval);
      new Notification("It's time for relax!!!");
    }
  }, 1000);

  Notification.requestPermission();
}

const showTime = computed(() => {
  let time = timeLeft.value;
  let hours = Math.floor(time / 60 / 60).toString();
  time -= Number(hours) * 60 * 60;
  let minutes = Math.floor(time / 60).toString().padStart(2, '0');
  let seconds = (time % 60).toString().padStart(2, '0');
  if (hours !== '0') {
    return `${hours}:${minutes}:${seconds}`;
  } else {
    return `${minutes}:${seconds}`;
  }
})
</script>

<template>
  <div class="d-flex align-items-center justify-content-center h-100">
    <div v-if="!timerIsRunning" class="d-flex flex-wrap justify-content-center">
      <button class="btn btn-success m-1" @click="startTimer(60)">
        Start 45 min sprint
      </button>
      <button class="btn btn-success m-1"  @click="startTimer(90)">
        Start 90 min sprint
      </button>
    </div>
    <div v-else class="d-flex align-items-center justify-content-center">
      <span class="display-1">{{showTime}}</span>
    </div>
  </div>
</template>

<style scoped>

</style>

<script setup>
import { ref } from 'vue'
import ListData from './components/ListData.vue'

const dialogIsVisible = ref(false)
const animatedBlock = ref(false)
const parIsVisible = ref(false)
const usersAreVisible = ref(false)
const enterInterval = ref(null)
const leaveInterval = ref(null)

const showUsers = () => {
  usersAreVisible.value = true
}

const hideUsers = () => {
  usersAreVisible.value = false
}

const showDialog = () => {
  dialogIsVisible.value = true
}
const hideDialog = () => {
  dialogIsVisible.value = false
}

const animateBlock = () => (animatedBlock.value = !animatedBlock.value)
const toggleParagraph = () => (parIsVisible.value = !parIsVisible.value)

const beforeEnter = (el) => {
  console.log('beforeEnter')
  console.log(el)
  el.style.opacity = 0
}

const enter = (el, done) => {
  console.log('Enter')
  console.log(el)
  let round = 1
  enterInterval.value = setInterval(() => {
    el.style.opacity = round * 0.01
    round++
    if (round > 100) {
      clearInterval(enterInterval.value)
      done()
    }
  }, 20)
}

const afterEnter = (el) => {
  console.log('afterEnter')
  console.log(el)
}

const beforeLeave = (el) => {
  console.log('beforeLeave')
  console.log(el)
  el.style.opacity = 1
}

const leave = (el, done) => {
  console.log('Leave')
  console.log(el)

  let round = 1
  leaveInterval.value = setInterval(() => {
    el.style.opacity = 1 - round * 0.01
    round++
    if (round > 100) {
      clearInterval(leaveInterval.value)
      done()
    }
  }, 20)
}

const afterLeave = (el) => {
  console.log('afterLeaver')
  console.log(el)
}

const enterCancelled = (el) => {
  console.log('enterCancelled')
  console.log(el)
  clearInterval(enterInterval.value)
}
const leaveCancelled = (el) => {
  clearInterval(leaveInterval.value)
  console.log('leaveCancelled')
  console.log(el)
}
</script>

<template>
  <router-view v-slot="slotProps">
    <transition name="fade-btn" mode="out-in"
      ><component :is="slotProps.Component"></component
    ></transition>
  </router-view>
</template>

<style>
* {
  box-sizing: border-box;
}
html {
  font-family: sans-serif;
}
body {
  margin: 0;
}
button {
  font: inherit;
  padding: 0.5rem 2rem;
  border: 1px solid #810032;
  border-radius: 30px;
  background-color: #810032;
  color: white;
  cursor: pointer;
}
button:hover,
button:active {
  background-color: #a80b48;
  border-color: #a80b48;
}
.block {
  width: 8rem;
  height: 8rem;
  background-color: #290033;
  margin-bottom: 2rem;
  /* transition: transform 0.3s ease-out; */
}
.container {
  max-width: 40rem;
  margin: 2rem auto;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 2rem;
  border: 2px solid #ccc;
  border-radius: 12px;
}
.animate {
  /* transform: translateX(-150px); */
  animation: slide-fade 0.3s ease-out forwards;
}

.fade-btn-enter-from,
.fade-btn-leave-to {
  opacity: 0;
}
.fade-btn-enter-active {
  transition: opacity 0.3s ease-out;
}

.fade-btn-leave-active {
  transition: opacity 0.3s ease-in;
}
.fade-btn-enter-to,
.fade-btn-leave-from {
  opacity: 1;
}

.route-enter-active {
  animation: slide-fade 0.4s ease-out;
}

.route-leave-active {
  animation: slide-fade 0.4s ease-in;
}

@keyframes slide-fade {
  0% {
    transform: translateX(0) scale(1);
  }
  70% {
    transform: translateX(-120px) scale(1.1);
  }
  100% {
    transform: translateX(-150px) scale(1);
  }
}
</style>

<script setup>
import { ref, onMounted } from 'vue'
const userInput = ref(null)
const users = ref(['Nissi', 'Eric', 'Theo', 'Gakyeli'])
const addUser = () => {
  const enteredName = userInput.value.value
  users.value.unshift(enteredName.charAt(0).toUpperCase() + enteredName.slice(1))
  userInput.value.value = ''
}

const removeUser = (user) => (users.value = users.value.filter((usr) => usr !== user))

onMounted(() => {
  userInput.value.focus()
})
</script>
<template>
  <transition-group tag="ul" name="user-list">
    <li v-for="user in users" :key="user" v-text="user" @click="removeUser(user)"></li>
  </transition-group>
  <div>
    <input type="text" name="" id="" ref="userInput" />
    <button @click="addUser">Add User</button>
  </div>
</template>

<style scoped>
ul {
  list-style: none;
  margin: 1rem 0;
  padding: 0;
}
li {
  border: 1px solid #ccc;
  padding: 1rem;
  text-align: center;
}

.user-list-enter-from {
  opacity: 0;
  transform: translateX(-30px);
}
.user-list-enter-active {
  transition: all 1s ease-out;
}
.user-list-enter-to,
.user-list-leave-from {
  opacity: 1;
  transform: translateX(0);
}

.user-list-leave-active {
  transition: all 1s ease-out;
  position: absolute;
}

.user-list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}

.user-list-move {
  transition: transform 0.8s ease;
}
</style>

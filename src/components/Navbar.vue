<script setup lang='ts'>
import MyButton from './MyButton.vue'
import { useStore } from '~/store/project'
const store = useStore()

function open() {
  console.log('open')
  store.count = store.count === 16 ? 36 : 16
}
</script>

<template>
  <div class="nav">
    <div class="title">
      memory
    </div>
    <div class="control">
      <MyButton @click="store.restart = true">
        Restart
      </MyButton>
      <MyButton @click="store.select = true">
        New Game
      </MyButton>
    </div>
  </div>
</template>

<style scoped>
.nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.title {
  font-size: 2.5rem;
  font-weight: bold;
}
.control {
  display: flex;
  gap: 1rem;
}

@media screen and (max-width: 720px) {
  .title {
    font-size: 1.5rem;
  }
}
</style>

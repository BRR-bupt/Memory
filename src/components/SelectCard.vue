<script setup lang='ts'>
import SelectButton from './SelectButton.vue'
import MyButton from './MyButton.vue'
import { useStore } from '~/store/project'
const store = useStore()

type Theme = 'Numbers' | 'Icons'
const gameTheme: Theme[] = ['Numbers', 'Icons']
const playerCount = [1, 2, 3, 4]
const gridSize = [16, 36]
</script>

<template>
  <div class="card">
    <div class="choice">
      <div class="title">
        Select Theme
      </div>
      <div class="button">
        <SelectButton
          v-for="(theme, index) in gameTheme" :key="index"
          :selected="store.theme === theme"
          @click="store.theme = theme"
        >
          {{ theme }}
        </SelectButton>
      </div>
    </div>

    <div class="choice">
      <div class="title">
        Numbers of Players
      </div>
      <div class="button">
        <SelectButton
          v-for="(count, index) in playerCount" :key="index"
          :selected="store.playerCount === count"
          @click="store.playerCount = count"
        >
          {{ count }}
        </SelectButton>
      </div>
    </div>

    <div class="choice">
      <div class="title">
        Grid Size
      </div>
      <div class="button">
        <SelectButton
          v-for="(size, index) in gridSize" :key="index"
          :selected="store.count === size"
          @click="store.count = size"
        >
          {{ `${size ** (1 / 2)} x ${size ** (1 / 2)}` }}
        </SelectButton>
      </div>
    </div>

    <MyButton @click="store.select = false">
      Start Game
    </MyButton>
  </div>
</template>

<style scoped>
.card {
  margin-left: auto;
  margin-right: auto;
  max-width: 580px;
  border-radius: 1rem;
  padding: 2rem 3rem;
  text-align: start;
  background-color: white;
}
.choice .title {
  font-size: 1.25rem;
  font-weight: bold;
  color: #7191a5;
}
.choice .button {
  display: flex;
  gap: 1rem;
  margin-top: 1rem;
  margin-bottom: 2rem;
}
</style>

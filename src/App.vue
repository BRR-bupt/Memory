<script setup lang="ts">
import Navbar from './components/Navbar.vue'
import GameArea from './components/GameArea.vue'
import SelectCard from './components/SelectCard.vue'
import { useStore } from '~/store/project'
const store = useStore()
// const select = ref(false)

const getStyle = computed(() => {
  if (store.select) {
    return {
      backgroundColor: '#152938',
      height: '100vh',
    }
  }
})
</script>

<template>
  <div class="main" :style="getStyle">
    <div v-if="store.select" class="game-select">
      <div class="title">
        memory
      </div>
      <div class="card-area">
        <SelectCard />
      </div>
    </div>
    <div v-else class="game-play">
      <Navbar />
      <div class="game-area">
        <GameArea />
      </div>
    </div>
  </div>

  <!-- <Footer /> -->
</template>

<style scoped>
.main {
  padding: 4rem 20rem;
  text-align: center;
}
/* .game-select {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
} */
.game-area {
  margin-top: 4rem;
}
.card-area {
  margin-top: 4rem;
}

.title {
  font-size: 2.5rem;
  font-weight: bold;
  color: white;
}

@media screen and (max-width: 1080px) {
  .main {
    padding: 4rem 8rem;
  }
}

@media screen and (max-width: 720px) {
  .main {
    padding: 2rem 2rem;
  }
}

@media screen and (max-width: 720px) {
  .title {
    font-size: 1.5rem;
  }
}
</style>

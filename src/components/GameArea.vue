<script setup lang='ts'>
import NumberCard from './NumberCard.vue'
import Confetti from './Confetti.vue'
import { useStore } from '~/store/project'
const store = useStore()
const win = ref('')

const nums = ref<number[]>([])
const curPlayer = ref(0)
const playerScore = ref<number[]>(new Array(store.playerCount).fill(0))
const moves = ref(0)

function init() {
  nums.value = new Array(store.count)
  for (let i = 0; i < store.count; i++)
    nums.value[i] = Math.floor(i / 2)

  nums.value.sort(() => { return Math.random() - 0.5 })
}

init()

let compareNums: number[] = []

const disabledArr = ref<number[]>([])
const abledArr = ref<number[]>([])

const count = ref(0)

const getStyle = computed(() => {
  return {
    gridTemplateColumns: `repeat(${store.count ** (1 / 2)}, minmax(0, 1fr))`,
  }
})

function recover(id: number) {
  count.value++
  compareNums.push(id)
  console.log(id)
}

function cover() {
  console.log(compareNums)
  abledArr.value = [...compareNums]
  nextTick(() => {
    abledArr.value = []
  })
}
const time = ref(0)
const timeStart = ref(false)
let timer: any

// 若有多个玩家，每次比较的时候，判断当前玩家
// 若比较相同，则当前玩家得分加一，若不同，轮次到下一位玩家
function compare() {
  console.log('比较')
  moves.value++
  setTimeout(() => {
    if (nums.value[compareNums[0]] === nums.value[compareNums[1]]) {
      disabledArr.value.push(...compareNums)
      playerScore.value[curPlayer.value]++
    }
    else {
      cover()
      if (curPlayer.value === store.playerCount - 1)
        curPlayer.value = 0

      else
        curPlayer.value++
    }

    if (disabledArr.value.length === store.count) {
      console.log('win')
      win.value = 'win'
      clearInterval(timer)
    }

    compareNums = []
    count.value = 0
  }, 500)
}

function restart() {
  win.value = 'win'
  clearInterval(timer)
  playerScore.value.fill(0)
  curPlayer.value = 0
  time.value = 0
  timeStart.value = false
  count.value = 0
  moves.value = 0
  disabledArr.value = []
  for (let i = 0; i < store.count; i++)
    abledArr.value[i] = i

  compareNums = []
  store.restart = false
  nums.value.sort(() => { return Math.random() - 0.5 })
  nextTick(() => {
    abledArr.value = []
  })
}

watch(() => store.restart, () => {
  if (store.restart)
    restart()
})

watch(count, () => {
  if (timeStart.value === false && count.value === 1 && store.playerCount === 1) {
    timeStart.value = true
    timer = setInterval(() => {
      time.value++
    }, 1000)
  }
  if (count.value === 2)
    compare()
})
</script>

<template>
  <div class="nums-group" :style="getStyle">
    <NumberCard
      v-for="(num, i) in nums" :id="i"
      :key="i"
      :num="num"
      :count="count"
      :mis-over="abledArr.includes(i)"
      :disabled="disabledArr.includes(i)"
      @recover="recover(i)"
    />
  </div>

  <div v-if="store.playerCount === 1" class="data">
    <div class="timer">
      <div class="title">
        Time
      </div>
      <div class="value">
        {{ time }} s
      </div>
    </div>
    <div class="movement">
      <div class="title">
        Moves
      </div>
      <div class="value">
        {{ moves }}
      </div>
    </div>
  </div>

  <div v-else class="data">
    <div v-for="(score, i) in playerScore" :key="i" class="player-score" :class="curPlayer === i ? 'cur-player' : ''">
      <div class="title">
        Player {{ i + 1 }}
      </div>
      <div class="value">
        {{ score }}
      </div>
    </div>
  </div>

  <Confetti :passed="win === 'win'" />
</template>

<style scoped>
.nums-group {
  margin-left: auto;
  margin-right: auto;
  text-align: center;
  display: grid;
  gap: 1rem;
  /* grid-template-columns: repeat(4, minmax(0, 1fr)); */
  max-width: 580px;
}
.data {
  margin-top: 4rem;
  margin-left: auto;
  margin-right: auto;
  /* text-align: center; */
  display: grid;
  gap: 2rem;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  max-width: 580px;
  font-size: 1.75rem;
  font-weight: bold;
}
.timer {
  /* border: 1px solid; */
  border-radius: 0.5rem;
  height: 5rem;
  background-color: #dfe7ec;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-left: 1rem;
  padding-right: 1rem;
}

.movement {
  border-radius: 0.5rem;
  height: 5rem;
  background-color: #dfe7ec;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-left: 1rem;
  padding-right: 1rem;
}
.player-score {
  border-radius: 0.5rem;
  height: 5rem;
  background-color: #dfe7ec;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-left: 1rem;
  padding-right: 1rem;
}

.cur-player {
  background-color: rgb(253, 162, 20);
}

.title {
  color: #7191a5;
  font-size: 1.25rem;
}
.value {
  color: #304859;
}
.cur-player .title {
  color: white;
}
.cur-player .value {
  color: white;
}

@media screen and (max-width: 720px) {
  .nums-group {
    gap: 0.5rem;
  }
}
</style>

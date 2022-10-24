<script setup lang='ts'>
const props = defineProps<{
  num: number
  id: number
  count: number
  misOver: boolean
  disabled: boolean
}>()
const emits = defineEmits<{
  (e: 'recover'): void
}>()

const over = ref(false)
watch(() => props.misOver, () => {
  if (props.misOver) {
    console.log('misOver')
    over.value = false
  }
})

const getStyle = computed(() => {
  if (props.disabled) {
    return {
      cursor: 'default',
      backgroundColor: '#bcced9',
    }
  }
  if (over.value) {
    return {
      cursor: 'default',
      backgroundColor: '#fda214',
    }
  }
})

function recover() {
  if (props.disabled || props.count === 2)
    return

  console.log('recover')
  over.value = true
  emits('recover')
}
</script>

<template>
  <div class="number-card" :style="getStyle" @click="recover">
    <div v-show="over" class="number">
      {{ num }}
    </div>
  </div>
</template>

<style scoped>
.number-card {
  width: 100%;
  aspect-ratio: 1;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 3rem;
  font-weight: bold;
  color: white;
  background-color: #304859;
  cursor: pointer;
  transition: 200ms;
  /* border: 1px solid; */
}
.number-card:hover {
  background-color: #55626b;
}

@media screen and (max-width: 720px) {
  .number-card {
    font-size: 1.5rem;
  }
}
</style>

<script setup lang='ts'>
const props = defineProps<{
  selected: boolean
}>()
const getStyle = computed(() => {
  if (props.selected) {
    return {
      backgroundColor: '#304859',
    }
  }
})
</script>

<template>
  <div class="my-button" :style="getStyle">
    <slot />
  </div>
</template>

<style scoped>
.my-button {
  /* width: fit-content; */
  flex: 1 1 0%;
  text-align: center;
  padding: 1rem 2rem;
  border-radius: 2rem;
  font-size: 1.25rem;
  font-weight: bold;
  color: white;
  background-color: #bcced9;
  cursor: pointer;
  transition: 200ms;
}
.my-button:hover {
  background-color: #92b8cf;
}
</style>

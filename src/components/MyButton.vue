<script setup lang='ts'>

</script>

<template>
  <div class="my-button">
    <slot />
  </div>
</template>

<style scoped>
.my-button {
  color: white;
  text-align: center;
  padding: 1rem 2rem;
  border-radius: 2rem;
  font-size: 1.25rem;
  font-weight: bold;
  background-color: rgb(253, 162, 20);
  cursor: pointer;
  transition: 200ms;
}
.my-button:hover {
  background-color: rgb(249, 185, 82);
}

@media screen and (max-width: 720px) {
  .my-button {
    font-size: 1rem;
    padding: 0.5rem 1rem;
  }
}
</style>

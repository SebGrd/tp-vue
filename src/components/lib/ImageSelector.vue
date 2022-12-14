<script setup>
import { defineProps } from "vue";

defineProps({
  value: {
    type: Boolean
  },
})
</script>

<template>
  <div class="image-selector">
    <p class="image-selector__title">Click the Wolf</p>
    <div class="image-selector__images">
      <img src="https://picsum.photos/seed/4/100" alt="img" @click="() => $emit('input', {target: { value: false}})">
      <img src="https://picsum.photos/seed/10/100" alt="img" @click="() => $emit('input', {target: { value: true}})">
    </div>
  </div>
</template>

<style scoped lang="scss">
.image-selector {
  &__title {
    font-weight: 700;
    margin-bottom: 0;
  }
  &__images {
    display: flex;
    justify-content: flex-start;
    align-items: center;
    gap: .5rem;
    img {
      border: 4px solid transparent;
      cursor: pointer;
      &:hover {
        border-color: cornflowerblue;
      }
      &.selected {
        border-color: cornflowerblue;
      }
    }
  }
}
</style>
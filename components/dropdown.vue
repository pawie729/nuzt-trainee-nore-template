<script setup lang="ts">
import { defineProps } from 'vue';

const props = defineProps<{
  selectedCategory: any;
  childCategories: any[];
}>();
</script>

<template>
  <div
    v-if="props.selectedCategory"
    class="absolute bg-white top-0 right-0 h-full w-full max-w-lg shadow z-50">
    <ul class="flex flex-col p-3 gap-2">
      <li v-for="child in props.childCategories" :key="child.id">
        {{ child.name }}
      </li>
    </ul>
  </div>
</template>

<style></style>
<script setup lang="ts">

const props = defineProps<{
  category: {
    name: string;
    id: string;
    parent_id: string;
    img1: string;
    img2: string;
    children: any[]; // Adjust the type of children as needed
    order_by: number;
    link: string;
  };
}>();

</script>

<template>
  <NuxtLink :to="category.link" class="flex items-center">
    <p>{{ category.name }}</p>
  </NuxtLink>
</template>

<style scoped>
</style>
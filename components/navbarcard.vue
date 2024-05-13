<script setup lang="ts">

const props = defineProps<{
  category: string;
}>();

const categorypath = computed(() => {
  return {
    path: `category/${props.category}`
  };
});

</script>

<template>
  <NuxtLink :to="categorypath.path" class="flex items-center tracking-wider space-x-2 text-sm">
    <p>{{ props.category }}</p>
  </NuxtLink>
</template>

<style scoped>
</style>
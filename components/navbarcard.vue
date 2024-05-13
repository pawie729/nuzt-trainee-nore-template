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
  <NuxtLink :to="categorypath.path" class="flex items-center">
    <p>{{ props.category }}</p>
  </NuxtLink>
</template>

<style scoped>
</style>
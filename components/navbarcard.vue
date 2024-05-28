<script setup lang="ts">

const props = defineProps<{
  category: {
    name: string;
    id: string;
    parent_id: string;
    img1: string;
    img2: string;
    children: any[];
    order_by: number;
    link: string;
  };
  selectedCategory: any;
  desktop?: boolean;
}>();

const categoryLink = computed(() => {
  return `/stores/${props.category.name}`;
});


</script>

<template>
    <div :class="[props.desktop ? 'fixed inset-0 flex items-center z-30' : 'flex flex-col']">

    

  <div class="flex space-x-1">
    <NuxtLink :to="categoryLink">
      <p>{{ category.name }}</p>
    </NuxtLink>

    <div v-if="props.desktop">
      <img 
      @mouseenter="$emit('mouseenter')" @mouseleave="$emit('mouseleave')"
      class="w-3 hover:w-4 hover:translate-y-3 -rotate-90 hover:-translate-x-1/2 duration-300 hover:rotate-0"
      src="/public/vectors/down-short-arrow-icon.svg">

    </div>

    <Dropdown
    v-if="props.desktop && props.selectedCategory && props.selectedCategory.id === props.category.id"
    :selectedCategory="props.selectedCategory"
    :childCategories="props.category.children"
    />

  </div>

  
</div>

</template>

<style scoped>
</style>
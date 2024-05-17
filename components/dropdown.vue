<script setup lang="ts">


interface Category {
  name: string;
  id: string;
  parent_id: string;
  img1: string;
  img2: string;
  children: Category[];
  order_by: number;
  link: string;
}

const props = defineProps({
  categories: Array,
  selectedCategory: Object as () => Category | null,
});

const isShown = ref(false);

const toggleDropdown = () => {
  isShown.value = !isShown.value;
};

const closeDropdown = () => {
  isShown.value = false;
};


</script>

<template>
  <div v-if="isShown" class="container absolute top-0 right-0 bg-white shadow-lg rounded-md z-50">


    <div @click="toggleDropdown" class="fixed inset-0 z-40 bg-black opacity-25"></div>

    <ul class="bg-white shadow-lg rounded-md">

      <li v-if="selectedCategory && selectedCategory.children" v-for="child in selectedCategory.children" :key="child.id"
      class="py-2 px-4 hover:bg-gray-100 cursor-pointer">
        {{ child.name }}
      </li>

    </ul>

  </div>

    
</template>

<style scoped>



</style>
<script setup lang="ts">


const categories = ref<string[]>([]);
const apiUrl = 'https://api.dilferclothing.com/dilferclothing.com/get_product_category_list';

const fetchCategories = async () => {
    try {
        const response = await fetch(apiUrl);
        const data = await response.json();

        categories.value = data.filter((category: { parent_id: any; }) => category.parent_id === "-").map((category: { name: string; }) => category.name);
    } catch (error) {
        console.error('Error fetching categories:', error);
    }
};

onMounted(fetchCategories);

</script>

<template>
  <div class="py-8">
    <header class="bg-white grid grid-cols-5">
    
      <!--Logo-->
      <div class="col-start-1 col-end-3">
        <a>
          <img alt="site-logo"
          src="">
        </a>
      </div>

      <!--Category Section-->
      <div class="col-start-3 col-end-6">
        <ul class="space-x-6 flex flex-row gap-5">
          <li>HOME</li>
          <li>NEW ARRIVALS</li>
          <Navbarcard
            v-for="category in categories"
            :key="category"
            :category="category"
          />

          <li>SALE</li>
          <li>GIFT VOUCHERS</li>
        </ul>
      </div>
    </header>
  </div>
</template>

<style>
</style>
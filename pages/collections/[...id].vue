<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';


const route = useRoute();
const endpointid = ref('');

const category = ref('');
const type = ref('');

onMounted(async () => {
  endpointid.value = route.params.id[0];
  try {
    if (endpointid.value === 'new-arrivals') {
      category.value = 'New Arrivals';
      type.value = 'new_arrivals'; 
    } else if (endpointid.value === 'Offers') {
      category.value = 'Offers';
      type.value = 'hot_offer'; 
    } else if (endpointid.value === 'trending'){
      category.value = 'Trending';
      type.value = 'trends';
    }
  } catch (error) {
    console.error('Error getting route path:', error);
  }
});


</script>

<template>
  <div>
    <Splash/>
    <CustomPage 
    page="Collections"
    :category="category" 
    :endpointid="endpointid" 
    :type="type"
    catpath=""
    order=""/>

  </div>
</template>

<style scoped>
</style>
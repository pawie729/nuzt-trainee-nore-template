<script setup lang="ts">

const props = defineProps({
  name: {
    type: String,
    required: true,
  },
  price: {
    type: String,
    required: true,
  },
  discount:{
    type: String,
  },
  instl: {
    type: String,
    required: true,
  },
  src: {
    type: String,
    required: true,
  },
})

</script>

<template>
  <div class="px-3 text-xs font-bold">
    <img alt="product-img"
    :src="props.src">
    <div class="p-3 space-y-2">
      <p >{{ name }}</p>

      <div class="flex space-x-3">
        <p>{{ price }}</p>
        <p class="text-neutral-500 line-through">{{ discount }}</p>
      </div>
      
      <div class="flex space-x-3">
        <p class="font-semibold text-neutral-400">{{ instl }}</p>
        <img class=" w-[50px]" alt="mintpay"
        src="https://greencoding.b-cdn.net/norareedfashion.com/front-end-asseta/mp.svg">
      </div>

    </div>
    
  </div>
</template>


<style scope>
</style>
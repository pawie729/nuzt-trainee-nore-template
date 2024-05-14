<script setup lang="ts">

const props = defineProps({
  name: String,
  price: String,
  instl: String,
  src: String,
})

</script>

<template>
  <div class="px-3 text-xs font-bold">
    <img alt="product-img"
    :src="props.src">
    <div class="p-3 space-y-2">
      <p>{{ name }}</p>
      <p>{{ price }}</p>
      <div class="flex space-x-1">
        <p class=" text-neutral-500">{{ instl }}</p>
        <img class=" w-[50px]" alt="mintpay"
        src="https://greencoding.b-cdn.net/norareedfashion.com/front-end-asseta/mp.svg">
      </div>
    </div>
    
  </div>
</template>


<style scope>
</style>
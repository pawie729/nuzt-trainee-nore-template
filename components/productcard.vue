<script setup lang="ts">

const props = defineProps({
  id: {
    type: Number,
    required: true,
  },
  name: {
    type: String,
    required: true,
  },
  price: {
    type: String,
    required: true,
  },
  oldprice: {
    type: String,
    default: ''
  },
  newprice: {
    type: String,
    default: ''
  },
  instl: {
    type: String,
    required: true,
  },
  src1: {
    type: String,
    required: true,
  },
  src2: {
    type: String,
    required: true,
  },
  masterTitle: {
    type: String,
    required: true,
  },
  percentage:{
    type: String,
    required: true,
  }
})

const router = useRouter();

const viewItem = () => {
  const itemURL = `/products/${props.masterTitle}/${props.id}`;
  router.push(itemURL);
}


</script>

<template>
  <div class="px-3 text-xs font-bold cursor-pointer focus:cursor-auto" @click="viewItem">
    <div class="relative pt-[151%] bg-gray-100">

      <img 
      alt="product-img"
      class="absolute top-0 left-0 w-full duration-300"
      :src="props.src2">

      <img 
      alt="product-img"
      class="absolute top-0 left-0 w-full z-10 duration-1000 hover:opacity-0" 
      :src="props.src1">

      <PercentageCircle
        :percentage="percentage"
      />
    </div>

    <div class="p-3 space-y-2">
      <p >{{ name }}</p>

      <div class="flex space-x-3">
        <p>{{ props.newprice ? newprice : price }}</p>
        <p v-if="props.newprice" class="text-gray-500 line-through">{{ oldprice }}</p>
      </div>
      
      <div class="flex space-x-3">
        <p class="font-semibold text-gray-400">{{ instl }}</p>
        <img class=" w-[50px]" alt="mintpay"
        src="https://greencoding.b-cdn.net/norareedfashion.com/front-end-asseta/mp.svg">
      </div>

    </div>
    
  </div>
</template>


<style scope>
</style>
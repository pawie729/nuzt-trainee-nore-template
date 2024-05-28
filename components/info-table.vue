<script setup lang="ts">

const props = defineProps({
  brand: String,
  code: String,
  stock_qty: String,
})

const isClosed = useState();

const showSection = ref('product');

</script>

<template>
  <div>
    <div>

<div class="flex flex-col bg-gray-100 border-b text-xs font-bold tracking-wide capitalize">

  <div class="pl-3 flex flex-row items-center justify-between">
    <p> Product details </p>

    <button class="p-3 px-6 border-l text-sm"
    @click="showSection = showSection === 'product' ? '' : 'product'">
      <span v-if="showSection !== 'product'">+</span>
      <span v-else>-</span>

    </button>

  </div>
  
  <div v-show="showSection === 'product'" class="product p-6 border-t bg-white leading-6 text-gray-600">

    <div class="flex flex-col space-y-2">

      <p>Brand : {{ brand }}</p>
      <p>Code : {{ code }}</p>
      <p>Availability : {{ stock_qty }}</p>

    </div>

  </div>

</div>

</div>

<div>
<div class="flex flex-col bg-gray-100 border-b text-xs font-bold tracking-wide capitalize">

  <div class="pl-3 flex flex-row items-center justify-between">

    <p> Delivery details </p>

    <button class="p-3 px-6 border-l text-sm"
    @click="showSection = showSection === 'delivery' ? '' : 'delivery'">
      <span v-if="showSection !== 'delivery'">+</span>
      <span v-else>-</span>

    </button>

  </div>
  
  <div v-show="showSection === 'delivery'" class="delivery p-6 border-t bg-white leading-6 text-gray-600">
    <ul class="text-[11px] list-inside list-disc">
      <li>Withing Colombo 2-3 DAYS</li>
      <li>Outside Colombo 3-5 DAYS</li>
    </ul>
    <p class="mt-6 leading-6">
      We will make every effort to complete deliveries within the allotted window of time. Unforeseen events might, however, prevent delivery from taking place as scheduled. We apologize in advance if this happens and will work to send your purchase as quickly as we can
    </p>

  </div>

</div>

</div>

<div>
<div class="flex flex-col bg-gray-100 border-b text-xs font-bold tracking-wide capitalize">

  <div class="pl-3 flex flex-row items-center justify-between">
    <div>
      <p> Size Guide </p>
    </div>
    
    <div>
      <button class="p-3 px-6 border-l text-sm"
      @click="showSection = showSection === 'size' ? '' : 'size'">
        <span v-if="showSection !== 'size'">+</span>
        <span v-else>-</span>

      </button>
    </div>
    

  </div>
  

  <div class="size p-6 border-t bg-white"
  v-show="showSection === 'size'">
    <img
    class="w-full"
    alt="size chart"
    src="https://greencoding.b-cdn.net/norareedfashion.com/front-end-asseta/size_chart2.svg"
    >

  </div>

</div>

</div>



  </div>
    
</template>

<style scoped>


</style>
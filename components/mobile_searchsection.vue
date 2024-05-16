<script setup lang="ts">

const emits = defineEmits(['close']);

const closeSearchSection = () => {
  emits('close');
};

</script>

<template>
  <div class="flex lg:hidden flex-col fixed inset-0 z-40 bg-white w-full h-full transform duration-500 origin-top scale-y-100 pointer-events-auto">
    <div class="flex pr-3 lg:pl-3 h-16 lg:h-20 items-center justify-between border-b-[1px] duration-300 opacity-100 delay-300">

      <div class="px-3">
        <img alt="site-logo"
        class="h-8"
        src="../public/vectors/brand-logo.svg">
      </div>

      <img alt="cross-icon"
      class="h-5"
      src="../public/vectors/cross-icon.svg"
      @click="closeSearchSection">  

    </div>

    <div class="pt-6 mb-4 px-4 border-b-[1px]">
      <input 
              type="search" 
              class=" 
              flex-grow
              border-none
              focus:outline-none
              text-xs
              font-semibold
              placeholder:text-sm" 
              placeholder="I am looking for..." 
              >
    </div>

    <div class="flex-grow mt-6 px-3 overflow-y-auto">

    </div>

    <div>

    </div>


  </div>
</template>

<style scoped>
</style>
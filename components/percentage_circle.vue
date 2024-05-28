<script setup lang="ts">

const props = defineProps({
  percentage: String,
})

</script>

<template>
  <div class="absolute top-2 right-2 flex items-center space-x-4 z-20">
    <div class="text-white bg-[#EC3326] h-10 font-semibold w-10 rounded-full flex items-center justify-center text-[11px]">
      <span>{{ percentage }}</span>
    </div>

    <div class="text-white bg-[#000000] h-10 font-semibold w-10 rounded-full flex items-center justify-center text-[11px]" style="display: none;">
      <span>NEW</span>

    </div>
  </div>

</template>

<style scoped>
</style>
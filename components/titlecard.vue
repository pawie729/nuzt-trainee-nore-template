<script setup lang="ts">

const props = defineProps({
  titlename: String,
})

</script>

<template>
  <div class="bg-white py-7 flex justify-center border-y-[1px]">
      <p class="font-bold text-5xl">{{ titlename }}</p>
  </div>
</template>

<style scoped>
</style>
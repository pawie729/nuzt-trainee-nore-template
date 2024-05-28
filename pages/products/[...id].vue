<script setup lang="ts">
    
    
    
const router = useRouter();

</script>

<template>
  <div>
    <Splash/>
    <ItemPage 
    :masterTitle="$route.params.master_title" :id="$route.params.id" />

  </div>
</template>

<style scoped>

</style>
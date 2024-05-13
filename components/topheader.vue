<script setup lang="ts">
</script>

<template>
  <div class="bg-neutral-300 grid grid-cols-2">
    <div class="py-2">
      <div class="col-start-1 col-end-2">
      </div>
      <div class="col-start-2 col-end-3">
        <!--SearchBar-->
        <div class="flex">
          <input 
            type="text" 
            class="py-3.5 px-5 
            rounded-es-full 
            rounded-ss-full 
            border-none 
            focus:outline-none 
            text-sm" 
            placeholder="I am looking for..." 
            style="width: 445px;">

            <div 
              class="p-2 flex 
              justify-center 
              rounded-ee-full rounded-se-full 
              text-white
                bg-white" 
              style="width: 64px;">

                <button 
                  type="button" 
                  class="focus:outline-none">
                    <img 
                    alt="search-icon" 
                    class="w-5 h-5" 
                    src="../public/Vectors/search-icon.svg">

                </button>
            </div> 
        </div>


 
      </div>

    </div>
  </div>
</template>

<style scoped>
</style>
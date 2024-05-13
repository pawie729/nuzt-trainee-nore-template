<script setup lang="ts">
</script>

<template>
  <div class="bg-neutral-300 py-2">

    <div class="grid grid-cols-2">

      <div class="col-start-1 col-end-2">
      </div>

      <div class="col-start-2 col-end-3 flex space-x-5">
        <!--Search Bar-->
        <div class="flex">
          <input 
            type="text" 
            class="py-3.5 px-5 
            rounded-es-full 
            rounded-ss-full 
            border-none 
            focus:outline-none 
            text-sm
             placeholder:text-sm font-bold" 
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
                    src="../public/vectors/search-icon.svg">

                </button>
            </div> 
        </div>

        <!--Icon Section-->
        <div class="flex text-xs font-bold gap-5 items-center" >

          <p>LKR</p>
          <img alt="profile-icon"
          class="w-5 h-5"
          src="../public/vectors/search-icon.svg">
          <img alt="fav-icon"
          class="w-4 h-4"
          src="../public/vectors/favorite-icon.svg">
          <div class="flex items-center">
            <img alt="cart-icon"
            class="w-6 h-6"
            src="../public/vectors/shopping-icon.svg">
            <p>( 0 )</p>
          </div>
          
          
        </div>

 
      </div>

    </div>
  </div>
</template>

<style scoped>
</style>
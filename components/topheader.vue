<script setup lang="ts">

const isMobileNavbarVisible = ref(false);

const isMobileSearchsectionVisiible = ref(false);

const toggleMobileNavbar = () => {
  isMobileNavbarVisible.value = !isMobileNavbarVisible.value;
};

const toggleSearchsection = () => {
  isMobileSearchsectionVisiible.value = !isMobileSearchsectionVisiible.value;
}

</script>

<template>
  <div class="flex p-3 py-4 lg:py-2 w-full 
  bg-white lg:bg-neutral-300
  sticky top-0 left-0 lg:relative z-40  items-center justify-between     
  shadow-lg lg:shadow-none">

    <div class="flex container mx-auto">

      <div class="flex lg:hidden space-x-1 ">
        <img 
            alt="profile-icon"
            class="w-5 h-5"
            src="../public/vectors/menu-icon.svg"
            @click="toggleMobileNavbar">

        <img alt="site-logo"
            class="h-6"
            src="../public/vectors/brand-logo.svg">

      </div>

      <div class="flex flex-grow justify-end space-x-5 ">

  
          <!--Search Bar-->
          <div class="px-5 py-4 hidden lg:flex flex-grow max-w-lg  bg-white 
          rounded-full
          items-center
          space-x-3">

          
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
              <div class="flex">

                  <button 
                    type="submit" 
                    class="focus:outline-none">
                      <img 
                      alt="search-icon" 
                      class="h-5" 
                      src="../public/vectors/search-icon.svg">

                  </button>
              </div> 
          </div>

          <!--Icon Section-->
          <div class="flex text-xs font-bold gap-5 items-center" >

            <p>LKR</p>
            <img alt="profile-icon"
            class="w-5 h-4"
            src="../public/vectors/profile-icon.svg">
            <img alt="fav-icon"
            class="w-4 h-4"
            src="../public/vectors/favorite-icon.svg">
            <div class="flex items-center">
              <img alt="cart-icon"
              class="w-6 h-6"
              src="../public/vectors/shopping-icon.svg">
              <p>( 0 )</p>
            </div>
            <img 
              alt="search-icon" 
              class="h-5 lg:hidden" 
              src="../public/vectors/search-icon.svg"
              @click="toggleSearchsection">
            
            
          </div>

      
      </div>

    </div>

  
  </div>

  <!-- Mobile Navbar -->
  <transition name="slide-left">
    <MobileNavbar v-if="isMobileNavbarVisible" @close="isMobileNavbarVisible = false"/>
  </transition>

  <!--Search Section-->
  <transition name="slide-top">
    <MobileSearchsection v-if="isMobileSearchsectionVisiible" @close="isMobileSearchsectionVisiible = false"/>
  </transition>

</template>

<style scoped>
.slide-left-enter-active, .slide-left-leave-active {
  transition: transform 0.3s ease;
}
.slide-left-enter, .slide-left-leave-to {
  transform: translateX(-100%);
}
</style>
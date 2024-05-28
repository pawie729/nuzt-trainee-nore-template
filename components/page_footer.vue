<script setup lang="ts">
</script>

<template>
  <div class="bg-white">
    <div class="py-12 border-t-[1px]">
      <div class="mx-auto px-3 grid lg:grid-cols-4 md:grid-cols-2 grid-cols-1 gap-8">

        <div>

          <NuxtLink to="/">
            <img alt="site-logo"
            class="h-12 mb-6"
            src="../public/vectors/brand-logo.svg">
          </NuxtLink>
          <p class="font-semibold text-xs text-gray-600 leading-6">
            We make it easy and affordable for you to be your own fashion icon. We are a modern chic label which is a global fashion company focused on the next generation of young women.
          </p>

        </div>

        <div class="lg:flex justify-center">

          <div>
            <p class="mb-6 font-bold">INFORMATION</p>

            <ul class="font-semibold text-xs text-gray-600 space-y-4">
              <li>About Us</li>
              <li>Privacy & Policy</li>
              <li>Terms & Conditions</li>
              <li>Delivery Details</li>
              <li>Return Policy</li>
            </ul>
            
          </div>

        </div>

        <div class="lg:flex justify-center">

          <div>
            <p class="mb-6 font-bold">FOLLOW US ON</p>

            <div class="font-semibold flex space-x-2">
              <NuxtLink to="https://web.facebook.com/NoraReedClothing/" target="_blank" rel="noopener">
                <img alt="fb-icon"
              class="w-8 h-8"
              src="../public/vectors/fb-icon.svg">
              </NuxtLink>
              
              <NuxtLink to="https://www.instagram.com/norareedfashion/" target="_blank" rel="noopener">
                <img alt="ig-icon"
              class="w-8 h-8"
              src="../public/vectors/ig-icon.svg">
              </NuxtLink>
              

              <NuxtLink to="https://api.whatsapp.com/send?phone=94760971358" target="_blank" rel="noopener">
                <img alt="wa-icon"
              class="w-8 h-8"
              src="../public/vectors/wa-icon.svg">
              </NuxtLink>
              

            </div>

          </div>

        </div>
      
        <div class="lg:flex justify-center">

          <div>
            <p class="mb-6 font-bold">CONTACT US</p>

            <ul class="font-semibold text-xs text-gray-600 space-y-4">
              <li>10.00 AM - 8.00 PM (Mon - Sun)</li>
              <li>No. 217 High Level Rd, Nugegoda</li>
              <li>(076) 097-1358</li>
              <li>Norareed2019@Yahoo.Com</li>

            </ul>

          </div>

        </div>

      </div>
    </div>

    <div class="flex justify-center py-5 border-t-[1px]">
      <p class="text-xs font-bold text-gray-500">© 2024 Nora Reed Fashion. All rights reserved. Powered by GPOS</p>
    </div>
    
  </div>
  
</template>

<style scoped>
</style>
<script setup lang="ts">


type ImageData = {
  img: string;
  data: { key: string; value: string }[];
};

const desktopImages = ref<ImageData[]>([]);
const mobileImages = ref<ImageData[]>([]);
const fetchImages = async () => {
  try {
    const response = await fetch('https://gcp-store-shared1.greencloudpos.com/norareedfashion.com/get_settings', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
      },
      body: JSON.stringify({ type: 'fileSectons' }),
    });

    const data = await response.json();
    const mainSliders = data.find((item: { value: string }) => item.value === 'Home Page - Main Sliders');
    if (mainSliders) {
      mainSliders.images.forEach((image: ImageData) => {
        if (image.data.some((item) => item.value === 'Desktop')) {
          desktopImages.value.push(image);
        } else if (image.data.some((item) => item.value === 'Mobile')) {
          mobileImages.value.push(image);
        }
      });
      console.log('Desktop Images:', desktopImages.value); // Log desktop images
      console.log('Mobile Images:', mobileImages.value); // Log mobile images
    }
  } catch (error) {
    console.error('Error fetching images:', error);
  }
};

onMounted(fetchImages)

</script>

<template>
  <div class="w-full h-full">


   

    <div class="desktop-slideshow hidden lg:block w-full h-full">

      <UCarousel
        :autoplay="true" :autoplay-speed="5000" :infinite="true">
        <div v-for="(image, index) in desktopImages" :key="`desktop-${index}`" class="slide">
          <img :src="image.img" class="w-full h-full object-cover" />
        </div>
      </UCarousel>
      

    </div>

    <div class="mobile-slideshow flex lg:hidden w-full h-full">

      <UCarousel
        :autoplay="true" 
        :autoplay-speed="5000"
        :infinite="true">

          <div 
          v-for="(image, index) in mobileImages" 
          :key="`mobile-${index}`" 
          class="slide">

            <img 
            :src="image.img" 
            class="w-full h-full object-cover" 
            />
            
          </div>

      </UCarousel>

    </div>

  </div>
</template>

<style scoped>

.slide {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
}

.slide img {
  max-width: 100%;
  max-height: 100%;
  object-fit: cover;
  transition: filter 1s ease-in-out;
}

</style>
<script setup lang="ts">


type ImageData = {
  img: string;
  data: { key: string; value: string }[];
};

const desktopImages = ref<ImageData[]>([]);
const mobileImages = ref<ImageData[]>([]);

const currentDesktopIndex = ref(0);
const currentMobileIndex = ref(0);

const fetchImages = async () => {
  try {
    const response = await fetch('https://gcp-store-shared1.greencloudpos.com/norareedfashion.com/get_settings', {

      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
      },
      body: JSON.stringify({
          type: 'fileSectons',

      }),

    });

    const data = await response.json();

    const mainSliders = data.find((item: { value: string }) => item.value === 'Home Page - Main Sliders');
    if (mainSliders) {
      mainSliders.images.forEach((image: ImageData) => {

        if (image.data.some((item) => item.value === 'Desktop')) {
          desktopImages.value.push(image);

        } else 

        if (image.data.some((item) => item.value === 'Mobile')) {
          mobileImages.value.push(image);
        }
      });
    }

  } catch (error) {

    console.error('Error fetching images:', error);
  }
}

onMounted(fetchImages);

const nextImage = () => {
  currentDesktopIndex.value = (currentDesktopIndex.value + 1) % desktopImages.value.length;
  currentMobileIndex.value = (currentMobileIndex.value + 1) % mobileImages.value.length;
};

onMounted(async () => {
  await fetchImages();
  setInterval(nextImage, 5000); // Change image every 5 seconds
});

</script>

<template>
  <div class="relative w-full h-full">

    <div class="desktop-slideshow hidden lg:block w-full h-full">

      <div v-for="(image, index) in desktopImages" :key="`desktop-${index}`" class="absolute inset-0 transition-opacity duration-1000" 
      :class="{ 'opacity-100': index === currentDesktopIndex }">

        <img 
        :src="image.img" 
        class="w-full h-full object-cover">
        
      </div>

    </div>

    <div class="mobile-slideshow flex lg:hidden w-full h-full">

      <div v-for="(image, index) in mobileImages" :key="`mobile-${index}`" class="absolute inset-0  opacity-0 transition-opacity duration-1000" 
      :class="{ 'opacity-100': index === currentMobileIndex }">

        <img 
          :src="image.img" 
          class="w-full h-full object-cover">
      </div>

    </div>

  </div>
</template>

<style scoped>
</style>
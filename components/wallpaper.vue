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
    }
  });
    }

  } catch (error) {

    console.error('Error fetching images:', error);
  }
}

onMounted(fetchImages);

</script>

<template>
  <div>
    <div class="desktop-slideshow">
      <img v-for="(image, index) in desktopImages" :key="`desktop-${index}`" :src="image.img">
    </div>
  </div>
</template>

<style scoped>
</style>
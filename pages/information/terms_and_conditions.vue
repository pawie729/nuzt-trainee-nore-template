<script setup lang="ts">


type ContentType = {
  h1: string;
  disc: string;
};

const content = ref<ContentType>({
  h1: '',
  disc: ''
});

const fetchData = async () => {
  try {
    const response = await fetch('https://gcp-store-shared1.greencloudpos.com/norareedfashion.com/get_settings', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ type: 'htmlTermsAndConditions' })
    });

    const data = await response.json();
    if (Array.isArray(data) && data.length > 0) {
      content.value = data[0];
    } else {
      console.error('Data format error: Expected an array with at least one element');
    }
  } catch (error) {
    console.error('Error fetching terms and conditions:', error);
  }
};

onMounted(fetchData );

</script>

<template>
  <div class="px-3 my-16 lg:mb-36 container mx-auto min-h-screen">
    <div v-html="content.disc"></div>
  </div>

</template>

<style scoped>
</style>
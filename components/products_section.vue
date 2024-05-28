<script setup lang="ts">


type Product = {
  id: number;
  sku: string;
  PRICE: number;
  DISCOUNT?: number;
  master_title: string;
  img: string;
  img2: string;
};

const props = defineProps({
  title: {
    type: String,
    required: true,
  },
  directedLink: {
    type: String,
    required: true,
  },
});

const products = ref<Product[]>([]);

const link = computed(() => {
  return `/collections/${props.directedLink}`;
});

const fetchProducts = async () =>{
  try{
    const response = await fetch('https://gcp-store-shared1.greencloudpos.com/norareedfashion.com/store_data',{

    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Accept': 'application/json',

    },

      body: JSON.stringify({
        currency_code: 'LKR',
        is_master: true,
        item_size: "",
        maxprice: 1000000,
        minprice: 0,
        order: 'NEWEST',
        p: 1,
        size: 8,
        type: props.directedLink,
      }),

    });

    const data = await response.json();
    products.value = data.list.map((product: any) => {
      const imgUrl = JSON.parse(product.variants).reduce((acc: string[], variant: any) => {
        if (variant.product_img_urls){
          acc.push(...variant.product_img_urls.map((img: any) => img.main_image));
        }
        return acc;
      }, []);

      const img2Url = imgUrl.length > 1 ? imgUrl[1] : imgUrl;
      return {
        ...product,
        name: product.master_title,
        img: imgUrl.length > 0 ? imgUrl[0] : '',
        img2: img2Url,
      };
    });

  }catch(error){

    console.error('Error fetching products:', error)

  }

  return products;
}

onMounted(fetchProducts);

</script>

<template>
  <div class="bg-white lg:px-3">

    <div class="bg-white py-7 flex justify-center border-y-[1px]">
      <p class="font-bold lg:text-5xl text-2xl">{{ title }}</p>

    </div>

    <div class="grid lg:grid-cols-4 md:grid-cols-3 grid-cols-2 py-12 gap-y-7">
      <Productcard 
        v-for="product in products" 
        :key="product.id" 
        :id="product.id" 
        :masterTitle="product.master_title" 
        :src2="product.img2" 
        :src1="product.img" 
        :name="product.master_title" 
        :price="`LKR ${product.PRICE}.00`" 
        :newprice="props.directedLink === 'hot_offer' && product.DISCOUNT ? `LKR ${(product.PRICE - product.DISCOUNT).toFixed(2)}.00` : ''" 
        :oldprice="product.DISCOUNT ? `LKR ${product.PRICE}.00` : ''" 
        :instl="props.directedLink === 'hot_offer' && product.DISCOUNT ? `LKR ${((product.PRICE - product.DISCOUNT)/3).toFixed(2)} x 3 with` : `LKR ${(product.PRICE/3).toFixed(2)} x 3 with`" 
        :percentage="props.directedLink === 'hot_offer' && product.DISCOUNT ? `- ${(product.DISCOUNT / product.PRICE * 100).toFixed(0)}%` : ''" 
      />
    </div>

    <div class="bg-white pb-12 px-3 flex justify-end">
      <NuxtLink :to="link">
        <button class="bg-black py-3 px-12">
          <p class="text-white text-xs font-semibold">View All</p>
        </button>
      </NuxtLink>
    </div>
  </div>
</template>

<style scope>
</style>
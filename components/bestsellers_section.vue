<script setup lang="ts">

type Product = {
  id: number;
  sku: string;
  PRICE: number;
  master_title: string;
  img: string;
};

const products = ref<Product[]>([]);

const fetchproducts = async () =>{
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
        type: 'trends',
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

      return {
        ...product,
        name: product.master_title,
        img: imgUrl.length > 0 ? imgUrl[0] : '',
      };
    });

  }catch(error){

    console.error('Error fetching products:', error)

  }

  return products;
}

onMounted(fetchproducts);

</script>

<template>
  <div class="bg-white py-12 px-3">
    <div class="grid grid-cols-4 gap-y-7">
      <Productcard 
        v-for="product in products"
        :key="product.id"
        :src="product.img"
        :name="product.master_title"
        :price="`LKR ${product.PRICE}.00`"
        :instl="`LKR ${(product.PRICE/3).toFixed(2)} x 3 with`"
    />
    </div>
  </div>
</template>

<style scope>
</style>
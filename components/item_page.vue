<script setup lang="ts">

const route = useRoute();

type Item = {
  id: number;
  cat_id: number;
  main_cat: string;
  sub_cat: string;
  brand: string;
  master_title: string;
  sp: number;
  code: string;
  stock_qty: number;
  discount: number;
  path: string;
  long_description: string;
  img: string;
  var_val1: string;
};

//id, main_cat, sub_cat, master_title, sp, stock_qrt, code, 


const product = ref<Item | null>(null);


const fetchItem = async () => {
  try {
    const response = await fetch('https://gcp-store-shared1.greencloudpos.com/norareedfashion.com/item', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
      },
      body: JSON.stringify({
        id: route.params.id,
        is_master: true,
        currency_code: 'LKR',
      }),
    });

    const data = await response.json();

    if (data && data.item && data.item.length > 0) {
      const itemData = data.item[0];
      product.value = {
        id: itemData.id,
        cat_id: itemData.cat_id,
        main_cat: itemData.main_id,
        sub_cat: itemData.sub_cat,
        brand: itemData.brand,
        master_title: itemData.master_title,
        sp: itemData.sp,
        code: itemData.code,
        stock_qty: itemData.stock_qty,
        discount: itemData.discount,
        path: itemData.path,
        long_description: itemData.long_description,
        img: itemData.img_urls[0]?.main_image || '',
        var_val1: itemData.var_val1,
      };
    }
  } catch (error) {
    console.error('Error fetching item:', error);
  }
};

onMounted(fetchItem);

</script>

<template>
  <div class="bg-white">

    <nav class="px-3 py-5 border-b-[1px] border-gray-100">
      <div class="flex container mx-auto items-center text-gray-500 font-bold text-xs space-x-1">
        <NuxtLink to="/">
          <p>Home</p>
        </NuxtLink>
        <p>|</p>
        <p>{{ product?.main_cat }}</p>
        <p>|</p>
        <p>{{ product?.sub_cat }}</p>
        <p>|</p>
        <p>{{ product?.master_title }}</p>
      </div>
    </nav>


    <div class="px-3 container mx-auto my-12">

      <div class="grid lg:grid-cols-2 grid-cols-1">

        <div class="flex space-x-2 lg:space-x-4">

          <div class="flex flex-col space-y-2 lg:space-y-4">

          </div>
          <div class="w-full flex-grow">

          </div>

        </div>

        <div>

          <p class="text-xl lg:text-4xl uppercase mb-6 font-bold">
            {{ product?.master_title ?? 'Loading...' }}
          </p>

          <p class="text-lg lg:text-3xl font-semibold pb-2">
            LKR {{ product?.sp?.toFixed(2) ?? '0.00' }}
          </p>

          <p class="text-[16px] font-semibold text-gray-500 mt-2 flex items-center space-x-1 flex-wrap mb-8">

            <span>LKR {{ (product?.sp ? (product.sp / 3).toFixed(2) : '0.00') }} x 3 with </span>

            <img class=" w-[50px]" alt="mintpay"
            src="https://greencoding.b-cdn.net/norareedfashion.com/front-end-asseta/mp.svg">

          </p>

          <p class="text-md font-bold mb-4">
            Code : {{ product?.code ?? 'N/A' }}
          </p>

          <p class="text-md font-bold mb-12">
            Availability : 
            
            <span class="text-red-600" v-if="product?.stock_qty === 0">
              Out of Stock
            </span>
            <span class="text-amber-500" v-else-if="product && product?.stock_qty > 0 && product?.stock_qty <= 5">
              Hurry! Only {{ product?.stock_qty }} left in stock
            </span>
            <span class="text-green-600" v-else>In Stock</span>

          </p>

          <div>
            <p class="">  {{ product?.long_description ?? 'Description not available.' }} </p>
          </div>

          <div class="mt-6">

            <p class="capitalize text-sm font-semibold mb-3">
              Size :
            </p>

            <div class="flex flex-wrap">
              <button class="bg-black border-black text-white uppercase text-xs font-semibold px-6 py-4 lg:px-10 border mr-2 lg:mr-4 mb-2 lg:mb-4 duration-300 hover:bg-black hover:text-white">
                {{ product?.var_val1 ?? 'N/A' }}
              </button>
              
            </div>
          </div>

          <!-- <p class="text-sm mt-3 text-green-600 font-semibold overflow-hidden duration-200 transform origin-top h-0 scale-y-0">
            You have selected the maximum quantity available at the moment. 
          </p> -->

          <!-- Add to Cart section -->

          <div class="flex items-center my-6 space-x-4">

            <div class="flex items-center justify-between space-x-6 px-6 py-4 border">

              <div class="flex items-center space-x-3">
                
                <button title="decrase" 
                class="text-xl font-bold cursor-not-allowed pointer-events-none">
                 - 
                </button>

                <span>1</span>

                <button title="increase" 
                class="text-lg font-semibold">
                   + 
                </button>
              </div>

            </div>

            <button class="cart_btn text-xs font-semibold text-white px-6 py-[22px] tracking-wide flex-grow active:bg-white active:text-black duration-500 bg-black">
              <span> Add To Cart </span>
              <span class="text-black line-through" style="display: none;">
                Out of Stock
              </span>

            </button>
            
            <button class="py-5 px-4 border duration-300" title="wishlist"><span class="text-lg"><div class="h-4"><svg class="h-full" viewBox="0 0 41 37" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M37.5 3.1C35.4 1 32.9 0 29.8 0C28.1 0 26.5 0.4 24.9 1.2C23.3 2.1 21.8 3.3 20.3 5.1C18.8 3.3 17.3 2 15.7 1.2C14.1 0.4 12.5 0 10.8 0C7.7 0 5.2 1 3.1 3.1C1 5.2 0 7.8 0 10.9C0 14.8 1.5 18.4 4.6 22C7.6 25.5 11.2 29.2 15.3 33L18 35.5C18.3 35.8 19.2 36.3 20.3 36.3C21.4 36.3 22.3 35.8 22.6 35.5L25.3 33C29.4 29.2 33 25.5 36 22C39.1 18.4 40.6 14.8 40.6 10.9C40.6 7.8 39.6 5.3 37.5 3.1ZM35.3 16.6C34 18.7 32.5 20.8 30.7 22.8C28.9 24.8 27 26.7 25.1 28.4C23.2 30.1 21.6 31.6 20.3 32.7C19 31.6 17.4 30.1 15.5 28.4C13.6 26.7 11.7 24.8 9.9 22.8C8.1 20.8 6.6 18.7 5.3 16.6C4 14.5 3.4 12.5 3.4 10.6C3.4 8.4 4.1 6.6 5.5 5.2C6.9 3.8 8.7 3.1 10.8 3.1C12.4 3.1 14 3.6 15.5 4.6C17 5.6 18.2 6.9 19.1 8.6C19.3 8.9 19.5 9.1 19.7 9.2C19.9 9.3 20.1 9.4 20.3 9.4C20.5 9.4 20.7 9.3 20.9 9.2C21.1 9.1 21.3 8.9 21.5 8.6C22.4 6.9 23.6 5.6 25.1 4.6C26.6 3.6 28.2 3.1 29.8 3.1C31.9 3.1 33.7 3.8 35.1 5.2C36.5 6.6 37.2 8.4 37.2 10.6C37.2 12.6 36.6 14.6 35.3 16.6Z" fill="black"></path></svg></div></span>
            </button>
          </div>

          
          <!-- Table -->
          <div class="mt-12 border">
            <InfoTable
            :brand="product?.brand ?? 'N/A'"
            :code="product?.code ?? 'N/A'"
            :stock_qty="product?.stock_qty && product.stock_qty > 0 ? 'In Stock' : 'Out of Stock'"/>
          </div>

        </div>

      </div>

    </div>

    <div class="px-3 py-12 lg:py-24 bg-gray-100">

      <div class="container mx-auto">
        <p class="text-xl lg:text-3xl font-bold mb-2">
          RELATED PRODUCTS

        </p>
        <p class="text-sm leading-7 font-medium tracking-wide max-w-screen-sm mb-12">
          Discover more products you'll love! Explore our related products section 

        </p>

        <div class="relative">
          <button class="absolute top-1/3 transform lg:-translate-x-1/2 z-10 left-0 bg-white h-10 w-10 flex lg:w-12 lg:h-12 items-center justify-center">


          </button>


          <button class="absolute top-1/3 transform lg:translate-x-1/2 z-10 right-0 bg-white h-10 w-10 flex lg:w-12 lg:h-12 items-center justify-center">



          </button>


          <section>
            <ol>
              <li>

              </li>
            </ol>
          </section>

        </div>
      
      </div>

    </div>

  </div>
</template>

<style scoped>
</style>
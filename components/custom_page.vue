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

const props = defineProps<{
  page: string;
  category: string;
  endpointid: string;
  type: string;
  catpath: string;
  order: String;
  
}>();


const products = ref<Product[]>([]);

const order = ref(props.order || 'NEWEST');

const fetchProducts = async () => {

  try {
    const response = await fetch('https://gcp-store-shared1.greencloudpos.com/norareedfashion.com/store_data', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
      },
      body: JSON.stringify({
        p: 1,
        size: 24,
        order: order.value,
        maxprice: 1000000,
        minprice: 0,
        brand: "",
        is_master: true,
        type: props.type || '',
        cat_path: props.catpath || '',
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

  } catch (error) {
    console.error('Error fetching products:', error);
  }
  return products;
};

onMounted(fetchProducts);
watch(() => [props.type, props.catpath, order.value], fetchProducts);


const isFilterOn = ref(false);

const toggleFilter = () => {
  isFilterOn.value = !isFilterOn.value;
};



</script>

<template>
  <div class="bg-white">

    <nav class="px-3 py-5 border-b-[1px] border-gray-100">

      <div class="flex container mx-auto items-center text-gray-500 font-bold text-xs space-x-1">
        <NuxtLink to="/">
          <p>Home</p>
        </NuxtLink>
        <p>|</p>
        <p>{{ props.page }}</p>
        <p>|</p>
        <p>{{ props.category }}</p>
      </div>

    </nav>

    <div class="py-12">


      <div class="flex flex-col lg:flex-row lg:items-center justify-between container mx-auto lg:mb-10 mb-6 text-xs font-semibold">

        <div class="flex flex-row mt-6 lg:mt-0 items-center space-x-3 ">

          <div class="outline-none focus:outline-none bg-white px-3  duration-300 flex border-[1px]">

            <select class="flex h-full outline-none p-3 gap-2" v-model="order">
              <option value="NEWEST">Newest</option>
              <option value="POPULARITY">Popularity</option>
              <option value="PRICE_LOW_TO_HIGH">Price, low to high</option>
              <option value="PRICE_HIGH_TO_LOW">Price, high to low</option>
              
            </select>

          </div>

          <button 
          @click="toggleFilter"
          class="border-[1px] px-12 py-3 relative capitalize duration-300">
            Filter
          </button>


        </div>
      </div>

      <section>

        <div class="container mx-auto grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-3 lg:gap-6">
          <Productcard 
            v-for="product in products"
            :key="product.id"
            :src="product.img"
            :name="product.master_title"
            :price="`LKR ${product.PRICE}.00`"
            :discount="props.catpath === '' && product.DISCOUNT ? `LKR ${(product.PRICE - product.DISCOUNT).toFixed(2)}.00` : ''"
            :instl="`LKR ${(product.PRICE/3).toFixed(2)} x 3 with`" />



        </div>

        <div class="flex items-center justify-center mt-12">

          <div class="flex items-center space-x-2">
            <button>
              <img
              alt="previous-icon"
              class="transform rotate-180"
              src="/public/vectors/previous-icon.svg">
            </button>

            <button class=" aspect-square w-8 border-[1px] bg-black text-white duration-300"> 
              1
            </button>

            <button class="aspect-square w-8 border-[1px] bg-white hover:bg-black hover:text-white duration-300">
              2
            </button>

            <button>
              <img
              alt="next-icon"
              src="/public/vectors/next-icon.svg">
            </button>

          </div>

        </div>



      </section>

    </div>

    


  </div>

  <!--Filter Pane-->
  <transition name="slide-right">
    <Filterpane v-if="isFilterOn" @close="isFilterOn = false"/>
  </transition>

</template>

<style scoped>



.slide-right-enter-active, .slide-right-leave-active {
  transition: transform 0.5s ease-in-out;
}
.slide-right-enter, .slide-right-leave-to {
  transform: translateX(100%);
}


</style>

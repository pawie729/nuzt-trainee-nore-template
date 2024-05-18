<script setup lang="ts">

// Define the type for a category
interface Category {
  name: string;
  id: string;
  parent_id: string;
  img1: string;
  img2: string;
  children: Category[];
  order_by: number;
  link: string;
}



// Define a reactive object to hold the category tree
const variables = reactive<{ categories: Category[] }>({
  categories: []
});

// Function to fetch categories from the API and organize them into a category tree
const get_categories = async () => {
  try {
    const response = await fetch('https://api.dilferclothing.com/dilferclothing.com/get_product_category_list');
    const data = await response.json();

    let _categories: Category[] = [];
        for (let index = 0; index < data.length; index++) {
            if (data[index].parent_id === '-') {
              // Create a new category object for the root level
              let _menu: Category = {
                    name: data[index].name,
                    id: data[index].id,
                    parent_id: data[index].parent_id,
                    img1: data[index].img_url1,
                    img2: data[index].img_url2,
                    children: [],
                    order_by: data[index].order_by,
                    link: `/categories/${data[index].name.replaceAll(' ', '-')}/`
                };
                // Push the root category to the temporary array
                _categories.push(_menu);
            } else {
              // Find the parent category in the existing _categories array
                for (let j = 0; j < _categories.length; j++) {
                    if (_categories[j].id === data[index].parent_id) {
                      // Create a new category object for the child category
                      let _menu: Category = {
                            name: data[index].name,
                            id: data[index].id,
                            parent_id: data[index].parent_id,
                            img1: data[index].img_url1,
                            img2: data[index].img_url2,
                            children: [],
                            order_by: data[index].order_by,
                            link: `/categories/${_categories[j].name.replaceAll(' ', '-')}/${data[index].name.replaceAll(' ', '-')}/`
                        };
                        // Push the child category to the children array of its parent
                        _categories[j].children.push(_menu);
                    }
                    for (let k = 0; k < _categories[j].children.length; k++) {                            
                        if(_categories[j].children[k].id == data[index].parent_id){
                            let _menu = {
                                name : data[index].name,
                                id : data[index].id,
                                parent_id : data[index].parent_id,
                                img1 : data[index].img_url1,
                                img2 : data[index].img_url2,
                                children : new Array,
                                order_by : data[index].order_by,
                                link : `/categories/${_categories[j].name.replaceAll(' ', '-')}/${_categories[j].children[k].name.replaceAll(' ', '-')}/${data[index].name.replaceAll(' ', '-')}/`
                            }
                            //Push the subchild category to new Array of its parent
                            _categories[j].children[k].children.push(_menu)
                  
                        }                            
                    }                
                    _categories[j].children.sort((a:any,b:any) => a.order_by - b.order_by);
                }
            }
        }

        _categories.sort((a, b) => a.order_by - b.order_by);
        variables.categories = _categories;
    } catch (error) {
        console.error('Error fetching categories:', error);
    }
};

// Call get_categories function on component mount
onMounted(get_categories);

const emit = defineEmits(['close']);

</script>

<template>
  <div class="flex fixed lg:hidden inset-0 z-50 h-full w-full opacity-100 pointer-events-auto">

    <div class="absolute inset-0 bg-black bg-opacity-75 h-full w-ful opacity-100 pointer-events-auto" @click="emit('close')"></div>

    <nav class="absolute top-0 bg-white h-full w-full flex flex-col max-w-md left-0 delay-300 duration-300">
    
      <!--Logo-->
      <div class="pr-3 border-b flex items-center justify-between h-16">

          <NuxtLink to="/">
            <img alt="site-logo"
            class="h-8 px-3"
            src="../public/vectors/brand-logo.svg">
          </NuxtLink>

          <img alt="cross-icon"
            class="h-8"
            src="../public/vectors/cross-icon.svg"
            @click="emit('close')">  

      </div>

      <!--Category Section-->
      <div class="flex-grow mt-6 px-3 overflow-y-auto">

        <ul class="flex flex-col space-y-4 list-inside font-bold text-xs tracking-wide">

          <li class="border-b pb-4 px-4">
            <NuxtLink to="/">HOME</NuxtLink>
          </li>

          <li class="border-b pb-4 px-4">
            <NuxtLink to="/collections/new-arrivals">NEW ARRIVALS</NuxtLink>
          </li>

          <Navbarcard
            v-for="category in variables.categories"
            class="border-b pb-4 px-4"
            :key="category.id"
            :category="category"
            selectedCategory=" "
            
          />

          <li class="border-b pb-4 px-4">
            <NuxtLink to="/collections/Offers">SALE</NuxtLink>
          </li>

        </ul>
      </div>
    </nav>

  </div>
</template>

<style scoped>


</style>
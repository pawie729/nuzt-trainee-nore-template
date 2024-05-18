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
const variables = reactive<{ categories: Category[]}> ({
  categories: [],
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

const isScrolled = ref(false);
const isHoveredOn = ref(false);
const selectedCategory = ref<Category | null>(null);

const handleScroll = () => {
  isScrolled.value = window.scrollY > 0;
};

const toggleDropdown = (category: Category) => {
  isHoveredOn.value = true;
  selectedCategory.value = category;
};

const toggleDropdownOff = () => {
  isHoveredOn.value = false;
  selectedCategory.value = null;
};

onMounted(() => {
  get_categories();
  window.addEventListener('scroll', handleScroll);
});

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll);
});

console.log(selectedCategory.value)


</script>

<template>
  <nav :class="[`px-3 py-4 hidden lg:block bg-white sticky top-0 left-0 z-30 border-b-[1px] border-gray-100`, isScrolled ? 'shadow-md' : ' shadow-none']">

    <div class="grid grid-cols-5 items-center">

      <!--Logo-->
      <div class="col-start-1 col-end-2">

        <NuxtLink 
        to="/">

          <img 
          alt="site-logo" 
          class="h-12" 
          src="../public/vectors/brand-logo.svg">

        </NuxtLink>

      </div>

      <!--Category Section-->
      <div class="flex col-span-4 col-end-6 justify-end">

        <ul class="flex flex-row text-xs font-bold space-x-4 items-center text-center">

          <li>
            <NuxtLink 
            to="/">
              HOME
            </NuxtLink>

          </li>

          <li>
            <NuxtLink 
              to="/collections/new-arrivals/">
              NEW ARRIVALS

            </NuxtLink>

          </li>

          <Navbarcard
            class="relative"
            v-for="category in variables.categories"
            :key="category.id"
            :category="category"
            :selectedCategory="selectedCategory">

          </Navbarcard>

          <li>

            <NuxtLink 
              to="/collections/Offers/">
              SALE

            </NuxtLink>

          </li>

        </ul>

      </div>
    </div>



    
  </nav>
</template>

<style>

</style>
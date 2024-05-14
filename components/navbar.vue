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

</script>

<template>
  <div class="bg-white  py-8">

    <header class="grid grid-cols-5">
    
      <!--Logo-->
      <div class="col-start-1 col-end-3">
        <a>
          <img alt="site-logo"
          src="">
        </a>
      </div>

      <!--Category Section-->
      <div class="col-start-3 col-end-6">
        <ul class="flex flex-row text-xs font-bold space-x-3">

          <li>NEW ARRIVALS</li>

          <Navbarcard
            v-for="category in variables.categories"
            :key="category.id"
            :category="category"
          />

          <li>GIFT VOUCHERS</li>
        </ul>
      </div>
    </header>

  </div>
</template>

<style>
</style>
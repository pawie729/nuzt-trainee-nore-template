<script setup lang="ts">



  /* creating categorty tree hint */


  //create reactive classs that contains all your varibles for this compoent/page

  let variables = reactive({
    category_tree: []
  })


  //create a function to get the category from the api make it in to the categry tree

  let get_categories = async()=>{


    $fetch(`https://api.dilferclothing.com/dilferclothing.com/get_product_category_list`,{
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body : {
        type : 'fileSectons'
      }
    }).then((result:any) => {

      console.log(result) // this will print out resposne of the api in the console.




      //the follwing nested for loops will loop through the reposne of the api in order to create the category tree. this is only a one way of doing this. try to understand this loop

        let _categories:any = []
        for (let index = 0; index < result.length; index++) {              
            if(result[index].parent_id == '-' ){
                let _menu = {
                    name : result[index].name,
                    id : result[index].id,
                    parent_id : result[index].parent_id,
                    img1 : result[index].img_url1,
                    img2 : result[index].img_url2,
                    children : new Array,
                    order_by : result[index].order_by,
                    link : `/categories/${result[index].name.replaceAll(' ', '-')}/`
                }
                _categories.push(_menu)
            }else{                    
                for (let j = 0; j < _categories.length; j++) {
                    if(_categories[j].id == result[index].parent_id ){
                        let _menu = {
                            name : result[index].name,
                            id : result[index].id,
                            parent_id : result[index].parent_id,
                            img1 : result[index].img_url1,
                            img2 : result[index].img_url2,
                            children : new Array,
                            order_by : result[index].order_by,
                            link : `/categories/${_categories[j].name.replaceAll(' ', '-')}/${result[index].name.replaceAll(' ', '-')}/`
                        }
                        _categories[j].children.push( _menu )                
                    }
                                                
                    for (let k = 0; k < _categories[j].children.length; k++) {                            
                        if(_categories[j].children[k].id == result[index].parent_id){
                            let _menu = {
                                name : result[index].name,
                                id : result[index].id,
                                parent_id : result[index].parent_id,
                                img1 : result[index].img_url1,
                                img2 : result[index].img_url2,
                                children : new Array,
                                order_by : result[index].order_by,
                                link : `/categories/${_categories[j].name.replaceAll(' ', '-')}/${_categories[j].children[k].name.replaceAll(' ', '-')}/${result[index].name.replaceAll(' ', '-')}/`
                            }
                            _categories[j].children[k].children.push(_menu)
                  
                        }                            
                    }                
                    _categories[j].children.sort((a:any,b:any) => a.order_by - b.order_by);
                }               
            }           
        }

        _categories.sort((a:any,b:any) => a.order_by - b.order_by);
        variables.categories = _categories
    



      //this will console log the category tree created using above functions with main categories and sub categories as its children
      console.log(variables.categories)



    })


  }



  onMounted(() => {
    //on mounted mean when page load. so when thwe page load this will run the get_actegories function
    get_categories()

  })








  // ***usage of reative classes are simplest thing on typescript and usage of loops are basic programming syntax of any language. so you should be  able to undersdtand and write those codes. it is a must have ability going forwaard..

  // site is is not responsive.. what ever the desing building. your approacg should be mobile first. make it responsive

  //horizontal scroll bars shouldnt be there

  //try to finish the home page by next meeting with the usage of get_settings and store_data api.. samples can be seen insdie network tab as we talked in the meeting





/* _________________________________________________________________________________________________________   */



























const categories = ref<string[]>([]);
const apiUrl = 'https://api.dilferclothing.com/dilferclothing.com/get_product_category_list';

const fetchCategories = async () => {
    try {
        const response = await fetch(apiUrl);
        const data = await response.json();

        categories.value = data.filter((category: { parent_id: any; }) => category.parent_id === "-").map((category: { name: string; }) => category.name);
    } catch (error) {
        console.error('Error fetching categories:', error);
    }
};

onMounted(fetchCategories);

</script>

<template>
  <div class="bg-white py-8">

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
            v-for="category in categories"
            :key="category"
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
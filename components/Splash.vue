<script setup lang="ts">

const useSplashStore = defineStore({
    id: 'splash',
    state: ()=> ({
        showSplashCard: true
    }),
    actions:{
        closeSplashCard(){
            this.showSplashCard = false;
        },
    }
});

const SplashCardStore = useSplashStore();

const closeOnBackgroundClick = (event: MouseEvent) => {
    if (event.target === event.currentTarget) {
        SplashCardStore.closeSplashCard();
    }
};

</script>

<template>
    <section @click="closeOnBackgroundClick" 
    v-show="SplashCardStore.showSplashCard"
    class="fixed z-50 inset-0 h-full w-full bg-black bg-opacity-40 backdrop-blur-md opacity-100 pointer-events-auto"
    >
            
        <div v-if="SplashCardStore.showSplashCard"
        class="fixed z-40 top-1/2 left-1/2 transform -translate-y-1/2 -translate-x-1/2 bg-white w-full shadow-xl max-w-[350px] lg:max-w-[450px] duration-1000 rounded-r-lg overflow-hidden"
        >

            <div class="relative">
                
                <button class="flex absolute bg-black h-6 w-6 top-2 right-2 justify-center items-center rounded-full">
                    <img 
                    @click.stop="SplashCardStore.closeSplashCard"
                    alt="cross-icon"
                    class="h-5"
                    src="../public/vectors/cross-icon-white.svg">  
                </button>

                <div class="block w-full">  
                    <img
                    class="w-full"
                    src="https://cdn.greencloudpos.com/norareedfashion.com/Popup/image-1710855112250.jpeg">  
                </div>

            </div>
        
        </div>
            
    </section>
</template>

<style scoped lang="scss">

</style>
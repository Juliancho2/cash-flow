<script>

import { defineAsyncComponent } from 'vue';
import Home from './components/Home.vue';
import SplashScreen from './components/SplashScreen.vue';


export default {
    components: {
    
    Home: defineAsyncComponent(() => {
        return new Promise((resolve) => {
            setTimeout(() => {
                resolve(import("./components/Home.vue"));
            }, 2500);
        });
    }),
    SplashScreen
}
}

</script>

<template>
    <!-- <SplashScreen/> -->
    <Suspense>
        <template #default>
            <Home/>
        </template>

        <template #fallback>
            <SplashScreen/>
        </template>

    </Suspense>
    
</template>


<style scoped>
html,
body,
.app {
    min-height: 100vh;
    max-height: 100vh;
    width: 100%;
    margin: 0;
    font-family: Arial, Helvetica, sans-serif;
    
}

* {
    --brand-green: #04b500;
    --brand-blue: #0689b0;
}
</style>

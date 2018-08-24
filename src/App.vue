<template>
  <div id="app">
    <XHeader title="Vue-cli3配置PWA" :left-options="leftOptions"></XHeader>
    <home-view></home-view>
  </div>
</template>
<script>
import { XHeader } from "vux";
import homeView from "./views/home.vue";
export default {
    data() {
        return {
         leftOptions:{
           showBack :false
         }
        };
    },
    components: {
        homeView,
        XHeader
    }
};
</script>

<style lang="scss">
* {
    padding: 0;
    margin: 0;
}
#app {
    font-family: "Avenir", Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
}
</style>

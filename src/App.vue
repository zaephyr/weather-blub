<template>
    <div id="app">
        <city-list />
    </div>
</template>

<script>
import CityList from './components/CityList.vue';

export default {
    name: 'App',
    components: {
        CityList,
    },
};
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>

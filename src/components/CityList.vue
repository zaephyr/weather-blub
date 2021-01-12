<template>
    <div>
        <city-name></city-name>
        <button>Add City</button>
        <div v-if="!cityList[0]">No cities</div>
        <ul v-else>
            <li v-for="city in cityList" :key="city">{{ city }}</li>
        </ul>
    </div>
</template>

<script>
import CityName from './CityName.vue';
export default {
    components: {
        CityName,
    },
    data() {
        return {
            cityList: [],
        };
    },
};
</script>

<style></style>

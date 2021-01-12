<template>
    <div>
        <city-name v-if="clicked" @clicked="clicked = false" :cityList="cityList"></city-name>
        <button v-else @click="clicked = true">Add City</button>
        <div v-if="!cityList[0]">No cities</div>
        <ul v-else>
            <li v-for="city in cityList" :key="city" @click="selectedCity = city">{{ city }}</li>
        </ul>
        <weather-info v-if="selectedCity" :city="selectedCity" :key="selectedCity" />
    </div>
</template>

<script>
import CityName from './CityName.vue';
import WeatherInfo from './WeatherInfo.vue';
export default {
    components: {
        CityName,
        WeatherInfo,
    },
    data() {
        return {
            selectedCity: '',
            clicked: false,
            cityList: [],
        };
    },
    methods: {
        addCity(val) {
            this.cityList.push(val);
        },
    },
};
</script>

<style></style>

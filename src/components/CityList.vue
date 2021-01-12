<template>
    <vue-pull-refresh :on-refresh="onRefresh" :config="pullConfig">
        <city-name
            v-if="clicked"
            @clicked="clicked = false"
            :cityList="cityList"
            @getList="cityList = $event"
        ></city-name>
        <button v-else @click="clicked = true">Add City</button>
        <div v-if="!cityList[0]">No cities</div>
        <ul v-else>
            <li v-for="city in cityObj" :key="city.name" @click="selectedCity = city.name">
                <span>{{ city.name }}</span
                ><span>{{ city.temperature }}&#8451;</span>
            </li>
        </ul>
        <weather-info
            v-if="selectedCity"
            :city="selectedCity"
            :key="selectedCity"
            :fetchData="fetchData(selectedCity)"
        />
    </vue-pull-refresh>
</template>

<script>
import VuePullRefresh from 'vue-pull-refresh';
import CityName from './CityName.vue';
import WeatherInfo from './WeatherInfo.vue';
export default {
    components: {
        CityName,
        WeatherInfo,
        VuePullRefresh,
    },
    data() {
        return {
            selectedCity: '',
            clicked: false,
            cityList: [],
            cityObj: [],
            pullConfig: {
                errorLabel: '',
                startLabel: '',
                readyLabel: '',
                loadingLabel: 'loading',
            },
        };
    },
    async mounted() {
        if (localStorage.cityList === undefined || localStorage.length == 0) {
            localStorage.cityList = JSON.stringify(this.cityList);
        } else {
            this.cityList = JSON.parse(localStorage.cityList);
            await this.fetchAllTemps();
        }
    },
    watch: {
        async cityList(newCityList) {
            await this.fetchAllTemps();
            localStorage.cityList = JSON.stringify(newCityList);
        },
    },
    methods: {
        async fetchData(val) {
            return await fetch(
                `https://api.openweathermap.org/data/2.5/weather?q=${val}&units=metric&appid=4c511407e418b92609a742841f1a2b44`
            )
                .then((res) => {
                    if (res.ok) {
                        return res.json();
                    } else {
                        throw new Error(`City ${val} doesn't exist`);
                    }
                })
                .then((data) => {
                    return {
                        name: data.name,
                        temperature: data.main.temp,
                        humidity: data.main.humidity,
                        description: data.weather[0].description,
                    };
                })
                .catch((err) => {
                    alert(err);
                });
        },
        async fetchAllTemps() {
            const promise = await Promise.all(
                this.cityList.map((city) => {
                    return this.fetchData(city);
                })
            );
            this.cityObj = promise;
        },
        onRefresh() {
            return new Promise((resolve) => {
                setTimeout(() => {
                    resolve(this.fetchAllTemps);
                }, 700);
            });
        },
    },
};
</script>

<style></style>

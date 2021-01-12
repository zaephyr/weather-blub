<template>
    <vue-pull-refresh :on-refresh="onRefresh" :config="pullConfig">
        <city-name
            v-if="clicked"
            @clicked="clicked = false"
            :cityList="cityList"
            @getList="cityList = $event"
        ></city-name>
        <button v-else @click="clicked = true" class="btn">Add City</button>
        <div class="container">
            <div v-if="!cityList[0]">No cities</div>

            <div v-for="(city, index) in cityObj" :key="city.name + city.temperature" class="city">
                <span style="margin: auto 0;" @click="selectedCity = city.name">{{ city.name }}</span
                ><span>{{ city.temperature }}&#8451; </span>
                <ion-icon name="trash-outline" class="trash" @click="removeCity(index)"></ion-icon>
            </div>

            <weather-info
                v-if="selectedCity"
                :city="selectedCity"
                :key="selectedCity"
                :fetchData="fetchData(selectedCity)"
                class="card"
            />
        </div>
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
                    this.selectedCity = '';
                }, 700);
            });
        },
        removeCity(index) {
            this.cityList.splice(index, 1);
            this.fetchAllTemps();
        },
    },
};
</script>

<style>
.btn {
    width: fit-content;
    cursor: pointer;
    padding: 0.5rem;
    border: 4px solid rebeccapurple;
    border-radius: 0.3rem;
    font-size: 0.75rem;
    background: rebeccapurple;
    color: white;
}

.container {
    margin-top: 2rem;
    display: flex;
    flex-direction: column;
    width: 100%;
    align-items: center;
}

.city {
    vertical-align: middle;
    width: 200px;
    display: flex;
    justify-content: space-between;
    align-content: center;
}

.trash {
    margin-left: 1rem;
    color: rebeccapurple;
}

.trash:hover {
    color: white;
    background-color: rebeccapurple;
}

.card {
    margin-top: 2rem;
}
</style>

<template>
    <div>
        <h2>{{ cityData.name }}</h2>
        <p>{{ cityData.description }}</p>
        <p>temperature: {{ cityData.temperature }}&#8451;</p>
        <p>humidity: {{ cityData.humidity }}</p>
    </div>
</template>

<script>
export default {
    props: ['city'],
    data() {
        return {
            cityData: {},
        };
    },
    async mounted() {
        await fetch(
            `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=4c511407e418b92609a742841f1a2b44`
        )
            .then((res) => res.json())
            .then((data) => {
                this.cityData = {
                    name: data.name,
                    temperature: data.main.temp,
                    humidity: data.main.humidity,
                    description: data.weather[0].description,
                };
            });
    },
};
</script>

<style></style>

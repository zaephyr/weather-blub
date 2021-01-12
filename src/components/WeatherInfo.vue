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
    props: { fetchData: { type: Promise } },
    data() {
        return {
            cityData: {},
        };
    },
    async mounted() {
        this.cityData = await this.fetchData;
    },
};
</script>

<style>
input {
    font-size: 1rem;
    height: 1.2rem;
    padding: 8px 0 8px 4px;
}
</style>

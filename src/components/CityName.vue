<template>
    <div>
        <div>
            <input type="text" v-model="city" />
            <button @click="closeCityName">Cancel</button>
            <button @click="passCityList">Finish</button>
        </div>
        <p v-if="error">{{ error }}</p>
    </div>
</template>

<script>
export default {
    props: {
        cityList: { type: Array },
    },
    data() {
        return {
            city: '',
            error: '',
        };
    },
    methods: {
        closeCityName() {
            this.$emit('clicked');
        },
        passCityList() {
            if (this.city && this.duplicateCity(this.city)) {
                this.cityList.push(this.cityName(this.city));
                this.$emit('getList', this.cityList);
                this.city = '';
                this.erorr = '';
                this.closeCityName();
            } else if (!this.city) {
                this.error = 'Empty input';
            } else {
                this.error = 'Duplicate city!';
            }
        },
        cityName(val) {
            val.toLowerCase();
            let city = val.charAt(0).toUpperCase() + val.slice(1);
            return city;
        },
        duplicateCity(val) {
            return !this.cityList.includes(this.cityName(val));
        },
    },
};
</script>

<style></style>

<template>
    <div>
        <div>
            <input type="text" v-model="city" />
            <div style="margin-top: 10px">
                <button @click="closeCityName" style="margin-right: 10px" class="btn">Cancel</button>
                <button @click="passCityList" class="btn">Finish</button>
            </div>
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
                this.city = '';
                this.error = '';
                this.$emit('getList', this.cityList);
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

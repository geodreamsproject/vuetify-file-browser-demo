<template>
    <div>
       <timeslide
            :axios="axiosInstance"
            :url="'/project/kasihan/sensor/virtual/device/WPC-1/data?time=ym'"
            :imgBaseURL="'http://localhost:8081/files'"
       />
    </div>
</template>

<script>
import Timeslide from './Timeslide.vue'
import axios from 'axios'

export default {
    components: {
        Timeslide
    },
    data () {
        return {
            axiosInstance: null
        };
    },
    async created () {
        this.axiosInstance = axios.create({ baseURL: 'http://localhost:8082' });
    }
};
</script>

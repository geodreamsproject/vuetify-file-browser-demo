<template>
    <div class="container">
        <div>
            <hooper
                :centerMode="true"
                :autoPlay="true"
                ref="carousel"
                @slide="updateCarousel"
            >
                <slide v-for="(item, indx) in items" :key="indx" :index="indx">
                    <img :src="imgBaseURL+item.y+imgQuery"/>
                </slide>
                <hooper-progress slot="hooper-addons"></hooper-progress>
            </hooper>
        </div>
        <div>
            <vue-slider
                v-model="value"
                :adsorb="true"
                :data="data"
                :marks="true"
            ></vue-slider>
        </div>
    </div>
</template>

<script>
import { Hooper, Slide, Progress as HooperProgress } from 'hooper';
import 'hooper/dist/hooper.css';
import VueSlider from 'vue-slider-component'
import 'vue-slider-component/theme/default.css'

export default {
    components: {
        Hooper,
        Slide,
        HooperProgress,
        VueSlider
    },
    props: {
        url: { type: String, default: "" },
        axios: { type: Function },
        // custom configuration for internal axios instance
        axiosConfig: { type: Object, default: () => {} },
        imgBaseURL: { type: String, default: "" },
        imgQuery: { type: String, default: "" }
    },
    data () {
        return {
            items: [],
            axiosInstance: null,
            value: null,
            data: []
        };
    },
    computed: {
        range () {
            let res = [];
            for (var i in this.items) {
                res.push(this.items[i].x)
            }
            return res;
        }
    },
    watch: {
        value () {
            this.$refs.carousel.slideTo(this.data.indexOf(this.value));
        }
    },
    methods: {
        updateCarousel (payload) {
            this.value = this.data[payload.currentSlide];
        },
        convertTime (t) {
            const monthNames = {
                '01': "Jan",
                '02': "Feb",
                '03': "Mar",
                '04': "Apr",
                '05': "May",
                '06': "Jun",
                '07': "Jul",
                '08': "Aug",
                '09': "Sep",
                '10': "Oct",
                '11': "Nov",
                '12': "Dec"
            };

            const k = t.toString().substring(4)
            const y = t.toString().substring(0, 4)
            return monthNames[k] + ' ' + y;
        }
    },
    async created () {
        let config = {
            url: this.url,
            method: "get"
        };
        let response = await this.axios.request(config);
        this.items = response.data[0].data;
        let res = [];
        for (var i in this.items) {
            res.push(this.convertTime(this.items[i].x))
        }
        this.data = res;
    }
};
</script>

<style>
  .container {
    width: 500px;
    margin: auto;
    padding: 20px 0;
  }
  .hooper {
    height: 200px;
    width: 500px;
  }
  .hooper-slide {
    padding: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
    color: #ffffff;
    border: 2px solid #fff;
    font-size: 30px;
    border-radius: 10px;
  }
</style>

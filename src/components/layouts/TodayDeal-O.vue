<template>
<div class="loader" v-show="loader">
    <img src="../../assets/loader.svg" />
</div>
<div class="container">
    <h4 class="text-center">
        🎁 Best Deals of the Day 🎁 <blink><img src="../../assets/discount.png" class="discount"> </blink>
    </h4>
    <div class="row row-cols-1 row-cols-md-5 g-4 pt-3">
        <div class="col" v-for="item in products.data" :key="item.id">
            <router-link :to="`/productdetails/`+ item.id" class="product">
                <div class="card">
                    <img :src="`${VUE_APP_IMAGE_URL}`+item.image" class="card-img-top p-4" alt="">
                    <div class="card-body">
                        <p class="card-title">{{item.name}}</p>
                        <div style="float:left">
                            <p><i class="fa fa-inr"></i> {{item.price}}</p>
                        </div>
                        <div style="float:right">
                            <p style="text-decoration:line-through;"><i class="fa fa-inr"></i> {{(item.price * 2).toFixed(2)}}</p>
                        </div>
                    </div>
                </div>
            </router-link>
        </div>
    </div>
</div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'TodayDeal',
    data() {
        return {
            products: [],
            VUE_APP_IMAGE_URL: process.env.VUE_APP_IMAGE_URL,
            loader: false
        }
    },
    methods: {
        async loadTodayDeal() {
            this.loader = true
            let user = localStorage.getItem('user-info') //user-info is local storage variable
            const token = JSON.parse(user).success.token // to get tokem from response
            // console.log(token);
            const headers = {
                "Content-type": "application/json; charset=UTF-8",
                "Authorization": 'Bearer ' + token
            }; //pass token

            let result = await axios.get("http://localhost:8000/api/productlist", {
                headers
            }); //get list of restaurant data
            this.products = result.data
            this.loader = false
            // console.log(this.products.data)
        },
        // async loadDetailProducts() {
        //     this.loader = true
        //     let user = localStorage.getItem('user-info'); //user-info is local storage variable
        //     const token = JSON.parse(user).success.token // to get tokem from response
        //     // console.log(token);
        //     const headers = {
        //         "Content-type": "application/json; charset=UTF-8",
        //         "Authorization": 'Bearer ' + token
        //     }; //pass token

        //     let result = await axios.get("http://localhost:8000/api/productDetails/" + this.$route.params.pro_id, {
        //         headers
        //     }); //get list of product category wise
        //     // console.log("result",result)

        //     this.product_detail = result.data
        //     this.loader
        //     // console.log("product", this.product_detail)
        // }
    },
    mounted() {
        this.loadTodayDeal()
        // this.loadDetailProducts()
    }
}
</script>

<style scoped>
img {
    height: 220px !important;
    width: 100% !important;
    object-fit: contain;
}

.card {
    border-radius: 20px;
}

.card:hover {
    background: #ffe0e9;
    transition: 0.3s;
}

.card-title {
    font-size: 12pt !important;
    font-weight: 500;
}

blink {
    -webkit-animation: 2s linear infinite condemned_blink_effect;
    /* for Safari 4.0 - 8.0 */
    animation: 1s linear infinite condemned_blink_effect;
    color: #4e7ab5;
}

.discount {
    height: 50px !important;
    width: 50px !important;
}

.product {
    text-decoration: none;
    color: inherit;
    /*Prevent changing link color*/
}

.loader {
    position: fixed;
    z-index: 1;
    display: flex;
    align-content: center;
    align-items: center;
    justify-content: center;
    height: 500px;
    width: 100%;
}

/* for Safari 4.0 - 8.0 */
@-webkit-keyframes condemned_blink_effect {
    0% {
        visibility: hidden;
    }

    50% {
        visibility: hidden;
    }

    100% {
        visibility: visible;
    }
}

@keyframes condemned_blink_effect {
    0% {
        visibility: hidden;
    }

    50% {
        visibility: hidden;
    }

    100% {
        visibility: visible;
    }
}
</style>

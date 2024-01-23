<template>
    <div class="product-page">
        <div>
            <Loading />
        </div>
        <div class="product-card">

            <!-- Product is not available -->
            <div class="product-not-available">
                <p class="text-no">This product is unavailable to show</p>
                <button class="btn-not-available">Next product</button>
            </div>

            <!-- Product is available -->
            <div class="product-available">

                <!-- Left -->
                <div class="product-card-left">
                    <img src="" alt="product" class="product-img">
                </div>

                <!-- Right -->
                <div class="product-card-right">
                    <h1 class="product-title">Hello</h1>
                    <div class="product-info">
                        <p class="product-category">hello</p>
                        <div class="product-rating">
                            <p class="product-rating-num"> / 5</p>
                            <div class="product-rating-circle">
                                <div class="circle"></div>
                            </div>
                        </div>
                    </div>
                    <hr />

                    <p class="product-desc"></p>
                    <div class="down">
                        <hr />
                        <h2 class="product-price"></h2>
                        <div class="btn-container">
                            <button class="btn">Buy Now</button>
                            <button class="btn btn-outline">Next Product</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import Loading from '../components/Loading.vue'

export default {
    name: 'ProductDisplayComponent',
    components: {
        Loading,
    },

    data() {
        return {
            index: 0,
            products: {},
            maxRates: [1, 2, 3, 4, 5],
            loading: false,
            isProductAvailable: false
        }
    },

    methods: {
        async API() {
            try {
                const url = `https://fakestoreapi.com/products/${this.index}`
                const response = await fetch(url)
                if (!response.ok) {
                    throw new Error('Request Failed')
                }
                const result = await response.json()
                return result
            } catch (error) {
                this.error = error.message
            }
        },

        async singleProduct() {
            this.loading = true
            if (this.index !== 20) {
                this.index++
            } else {
                this.index = 1
            }

            const data = await this.API()
            if (data.category === "men's clothing" || data.category === "women's clothing") {
                this.product = { data }
                this.isProductAvailable = true
            } else {
                this.isProductAvailable = false
            }

            this.loading = false
        }
    },

    mounted() {
        this.singleProduct()
    }
}

</script>

<style>

@import url('../assets/style/page.css')

</style>
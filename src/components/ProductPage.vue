<template>
    <div
        class="product-page" 
        :class="!isProductAvailable ? 'bg-gray' : products.data.category === 'men\'s clothing' ? 'bg-skyBlue' : 'bg-lightPurple'
    ">
        <div v-if="loading">
            <Loading />
        </div>
        <div class="product-card">

            <!-- Product is not available -->
            <div 
                class="product-not-available"
                :class="isProductAvailable ? 'display-none' : 'product-not-available'"
            >
                <p class="text-no">This product is unavailable to show</p>
                <button class="btn-not-available" @click="singleProduct()">Next product</button>
            </div>

            <!-- Product is available -->
            <div 
                class="product-available"
                :class="!isProductAvailable ? 'display-none' : 'product-available'"
            >

                <!-- Left -->
                <div class="product-card-left">
                    <img :src="products?.data?.image" alt="product" class="product-img">
                </div>

                <!-- Right -->
                <div class="product-card-right">
                    <h1 
                        class="product-title"
                        :class="products?.data?.category === 'men\'s clothing' ? 'color-navy' : 'color-darkPurple'"
                    >
                        {{ products?.data?.title }}
                    </h1>
                    <div class="product-info">
                        <p class="product-category">{{ products?.data?.category }}</p>
                        <div class="product-rating">
                            <p class="product-rating-rate">{{ products?.data?.rating?.rate }} / 5</p>
                            <div class="product-rating-circle">
                                <div 
                                    v-for="maxRate in maxRates"
                                    :key="maxRate"
                                    class="circle"
                                    :class="products?.data?.category === 'men\'s clothing' ? 'color-navy' : 'color-darkPurple'"
                                ></div>
                            </div>
                        </div>
                    </div>
                    <hr />

                    <p class="product-desc">{{ products?.data?.description }}</p>
                    <div class="down">
                        <hr />   
                        <h2 
                            class="product-price"
                            :class="products?.data?.category === 'men\'s clothing' ? 'color-navy' : 'color-darkPurple'"
                        >
                            ${{ products?.data?.price }}
                        </h2>
                        <div class="btn-container">
                            <button 
                                class="btn"
                                :class="products?.data?.category === 'men\'s clothing' ? 'font-white bg-navy' : 'font-white bg-darkPurple'"
                            >
                                Buy Now
                            </button>
                            <button
                                @click="singleProduct()"
                                class="btn btn-outline"
                                :class="products?.data?.category === 'men\'s clothing' ? 'border-navy' : 'border-darkPurple'"
                                
                            >
                                Next Product
                            </button>
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
    name: 'ProductPageComponent',
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
                this.products = { data }
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
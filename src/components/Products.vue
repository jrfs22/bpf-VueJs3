<script>
import Navbar from './navbar.vue'
import PriceSlider from './PriceSlider.vue'
import ProductList from './productList.vue'
export default{
    name: 'products',
    data: function () {
        return {
            harga1: 50
        }
    },
    props: [
        "product",
        "harga",
        "cart",
        "cartQty",
        "cartTotal",
        "status",
        "statusSlider"
    ],
    components: {
        Navbar,
        PriceSlider,
        ProductList
    },
    methods: {
        toggleSliderStatus: function () {
            this.$emit('toggle')
        },
        addItem: function (item) {
            this.$emit('add', item)
        },
        deleteItem: function (index) {
            this.$emit('delete', index)
        }
    }
}
</script>

<template>
    <h1>ID Shop</h1>
    <navbar 
      :cart="cart"
      :cartQty="cartQty"
      :cartTotal="cartTotal"
      @toggle-slide="toggleSliderStatus"
      @delete-item="deleteItem"
    />
    <price-slider :status="status" :harga="harga1"/>
    <product-list :product="product" :harga="harga1" @add-item="addItem"/>
</template>

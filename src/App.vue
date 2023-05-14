<script>
import {ref, onMounted} from 'vue'
import Navbar from './components/navbar.vue'
import PriceSlider from './components/PriceSlider.vue'
import ProductList from './components/productList.vue'
export default {
  name: "App",
  data: function(){
    return {
      harga: 50,
      cart: [],
      status: false
    }
  },
  components: {
    ProductList,
    Navbar,
    PriceSlider
  },
  setup() {
    const product = ref([])

    onMounted(() => {
      fetch('http://hplussport.com/api/products/order/price')
      .then(response => response.json())
      .then((data) => {
        product.value = data
      })
    })

    return {
      product
    }
  },
  computed: {
    cartTotal: function () {
      let sum = 0
      for (let key in this.cart){
        sum = sum + this.cart[key].product.price * this.cart[key].qty
      }
      return sum
    },
    cartQty: function () {
      let qty =0
      for(let key in this.cart){
        qty = qty + this.cart[key].qty
      }
      return qty
    }
  },
  methods: {
    toggleSliderStatus: function () {
      this.status = !this.status
    },
    addItem: function (product) {
      let productIndex
      let productExist = this.cart.filter(function(item, index){
        if(item.product.id == Number(product.id)){
          productIndex = index
          return true
        }else{
          return false
        }
      })

      if(productExist.length){
        this.cart[productIndex].qty++
      }else{
        this.cart.push({product: product, qty: 1})
      }
    },
    deleteItem: function(key){
      if(this.cart[key].qty > 1){
        this.cart[key].qty--
      }else{
        this.cart.splice(key, 1)
      }
    }
  }
}
</script>

<template>

  <div class="container mt-5">
    <h1>ID Shop</h1>
    <navbar 
      :cart="cart"
      :cartQty="cartQty"
      :cartTotal="cartTotal"
      @toggle="toggleSliderStatus"
      @delete="deleteItem"
    />
    <price-slider :status="status" :harga.sync="harga"/>
    <product-list :product="product" :harga="harga" @add="addItem"/>
  </div>
</template>
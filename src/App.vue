<script>
import {ref, onMounted} from 'vue'
import Products from "./components/Products.vue"
import Checkout from './components/Checkout.vue'
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
    Products,
    Checkout
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
    <checkout 
      :cart="cart"
      :cartTotal="cartTotal"
      :add="addItem"
      @delete="deleteItem"
    />
    <products  
      :cart="cart"
      :cartQty="cartQty"
      :cartTotal="cartTotal"
      :harga.sync="harga"
      :product="product"
      :status="status"
      @toggle="toggleSliderStatus"
      @add="addItem"
      @delete="deleteItem"
    />
  </div>
</template>
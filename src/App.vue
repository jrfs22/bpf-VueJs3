<script>
export default({
  name: 'App',
  data: function(){
    return {
      harga: 50,
      product: []
    }
  },
  components: {
    productList
  },
  mounted: function(){
    fetch('http://hplussport.com/api/products/order/price')
    .then(response => response.json())
    .then(data => {
      this.product = data
    }).catch(error => {
      console.error(error)
    })
  }
})
</script>

<template>
  <div class="container mt-5">
    <h1>ID Shop</h1>
    <product-list :product="product" :harga="harga" />
  </div>
</template>



<script>
    import Price from "./Price.vue"
    export default {
        name: 'product-list',
        components: {
            Price
        },
        props: ['product', 'harga'],
        computed: {
            showItem: function () {
                let max = this.harga;
                return this.product.filter(function(item){
                    return Math.trunc(item.price) <= max
                })
            }
        } 
    }
</script>

<template>
    <transition-group 
        name="fade" 
        tag="div" 
        enter-active-class="animate__animated animate__bounceInLeft" 
        leave-active-class="animate__animated animate__bounceInRight"
    >
    <div 
        class="row d-flex mb-3 align-items-center" 
        v-for="(item, index) in showItem"  
        :key="item.id"
        :data-index="index"
    >
        <div class="col-1 m-auto">
            <button class="btn btn-info" v-on:click="$emit('add', item)">+</button>
        </div>
        <div class="col-sm-4">
            <img :src="item.image" :alt="item.name" class="img-fluid">
        </div>
            <div class="col">
                <h2>{{ item.name }}</h2>
                <p>{{ item.description }}</p>
                <div class="h3">
                    <price :value="Number(item.price)"/> 
                </div>
            </div>
        </div>
    </transition-group>
</template>

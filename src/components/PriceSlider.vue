<script>
export default{
    name: 'price-slider',
    data: function () {
        return {
            maxAmount: 50
        }
    },
    props: ['status', 'harga'],
    computed: {
        statusSlider: function () {
            return this.status ? 'd-flex' : 'd-none'
        }
    }
}
</script>
<template>
    <transition 
        name="custom" 
        enter-active-class="animate__animated animate__fadeInDown" leave-active-class="animate__animated animate__fadeOutDown"
    >
        <div 
            v-if="status"
            class="form-inline mr-auto mb-3 d-flex align-items-center"
        >
            <label for="number" class="font-weight-bold mr-2">Max</label>
            <input 
                type="number" 
                id="number" 
                class="form-control w-25" 
                min="0"
                max="100"
                style="width: 60px; text-align: center;" 
                model="maxAmount"
                @change="$parent.$emit('update:harga', maxAmount)"
            >
            <input 
                type="range" 
                class="custom-range" 
                model="maxAmount"
                @input="$parent.$emit('update:harga', maxAmount)"
            >
        </div>
    </transition>
</template>

<style>
.fade-enter,
.fade-leave-to{
    opacity: 0;
}

.fade-enter-active,
.fade-leave-active{
    transition: all 1s ease-in-out;
}
</style>
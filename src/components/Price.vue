<script setup>
defineProps({
    value: Number,
    prefix: {
        type: String,
        default: '$'
    },
    precission:{
        type: Number,
        default: 2
    }
})
</script>

<template>
    <span>{{ prefix + Number.parseFloat(value).toFixed(precission) }}</span>
</template>

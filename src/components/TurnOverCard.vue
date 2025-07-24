<script setup>

defineProps({
   image: {
    type: String,
    required: true
   },

   isRotate: {
    type: Boolean,
    required: true
   },

   isMatched: {
    type: Boolean,
    required: true
   }
})


</script>
<template>
    <div class="card" @click="$emit('flip')" :class="{'rotate': isRotate, 'matched': isMatched}">

        <div class="card-rotation">

        <div class="card-front">
             <img :src="image" alt="front-image" class="card-img">
            </div>
        <div class="card-back"></div>

        </div>
    
    </div>
</template>

<style scoped>
@import './styles.css'
</style>
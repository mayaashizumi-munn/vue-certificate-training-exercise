<script setup lang="ts">
import { defineEmits } from 'vue'
const emits = defineEmits(['close'])
</script>

<template>
    <div id="movie-modal-wrapper">
        <div id="movie-modal-container">
            <div @click="$emit('close')">close</div>
            <p>Hi</p>
        </div>
    </div>
</template>

<style scoped>
#movie-modal-wrapper {
    position: absolute;
    width: 100%;
    height: 100%;
    backdrop-filter: blur(10px);
    left: 0;
    top: 0;

    display: flex;
}

#movie-modal-container {
    background-color: pink;
    min-width: 500px;
    margin: auto;
    padding: 24px;
}
</style>
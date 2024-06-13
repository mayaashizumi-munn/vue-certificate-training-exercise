<script setup lang="ts">
import { defineEmits } from 'vue'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { faClose } from '@fortawesome/free-solid-svg-icons'

const emits = defineEmits(['close'])
</script>

<template>
    <div id="movie-modal-wrapper">
        <div id="movie-modal-container" class="bg-gray-600">
            <button
                @click="$emit('close')" 
                class="close-icon"
            >
                <FontAwesomeIcon :icon="faClose" />
            </button>
            
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
    position: relative;
    min-width: 500px;
    margin: auto;
    padding: 24px;
    border-radius: 8px;
    color: white;
}

.close-icon {
    position: absolute;
    right: 24px;
}
</style>
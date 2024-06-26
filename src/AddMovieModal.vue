<script setup lang="ts">
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { faClose } from '@fortawesome/free-solid-svg-icons'

const emits = defineEmits(['close', 'addMovie'])

const name = defineModel('name')
const description = defineModel('description')
const image = defineModel('image')
const selectedGenres = defineModel('selectedGenres')
const isInTheatres = defineModel('isInTheatres')

const labelClass = 'block text-gray-700 text-sm font-bold mb-2'
const inputClass = 'shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline'

const genres = [
    'Comedy',
    'Horror',
    'Drama',
    'Action',
    'Childrens',
    'Romance',
    'Thriller'
]

const onFormSubmit = () => {
    emits('addMovie')
    emits('close')
}
</script>

<template>
    <div id="movie-modal-wrapper">
        <div id="movie-modal-container" class="bg-gray-300 shadow-md rounded px-8 pt-6 pb-8 mb-4">
            <button
                type="button"
                @click="$emit('close')" 
                class="close-icon"
            >
                <FontAwesomeIcon :icon="faClose" />
            </button>
            
            <form id="add-movie-form" @submit.prevent="onFormSubmit">
                <label for="name" :class="[labelClass]">Name *</label>
                <input type="text" :class="[inputClass]" id="name" name="name" required v-model="name">
                <br />

                <label for="description" :class="[labelClass]">Description</label>
                <textarea :class="[inputClass]" id="description" name="description" rows="3" v-model="description"></textarea>
                <br />

                <label for="image" :class="[labelClass]">Poster Image URL *</label>
                <input type="text" :class="[inputClass]" id="image" name="image" required v-model="image">
                <br />

                <label for="genres" :class="[labelClass]">Genres *</label>
                <select name="genres" id="genres" multiple required v-model="selectedGenres">
                    <option v-for="genre in genres" :value="genre">{{ genre }}</option>
                </select>
                <br />

                <label for="inTheatres" :class="[labelClass]">Is in theatres?</label>
                <input id="in-theatres-checkbox" type="checkbox" name="inTheatres" v-model="isInTheatres" />
                <br />

                <button type="submit" class="text-sm submit-movie-button">
                    Create
                </button>
            </form>
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
    padding: 44px 24px 32px 24px;
    border-radius: 8px;
}

.close-icon {
    position: absolute;
    right: 20px;
    top: 20px;
}

#add-movie-form {
    display: flex;
    flex-direction: column;
}

#in-theatres-checkbox {
    width: fit-content;
}

.submit-movie-button {
    background: rgb(0, 153, 255);
    border-radius: 8px;
    padding: 2px 0px;
    color: white;
}
</style>
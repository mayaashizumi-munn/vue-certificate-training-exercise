<script setup lang="ts">
import { items } from "./movies.json"
import { ref, Ref } from "vue"
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { faStar as faStarFull } from "@fortawesome/free-solid-svg-icons"
import { StarIcon } from "@heroicons/vue/24/solid"
import AddMovieModal from "./AddMovieModal.vue"

interface Movie {
    id: number
    name: string
    description: string
    genres: string[]
    image: string
    inTheaters: boolean
    rating: number | null
}

const movies: Ref<Movie[]> = ref(items)

const showAddMovieModal = ref(false)

const getFullOrEmptyStarColor = (starIndex: number, rating: number | null): string => {
    return rating && starIndex <= rating ? '#f5b642' : 'grey'
}

const onStarClick = (starNumber: number, movieIndex: number) => {
    if (starNumber !== movies.value[movieIndex].rating) {
        movies.value[movieIndex].rating = starNumber
    }
}

const getStarStyles = (rating: number | null): string => {
    return rating && rating > 0 ? 'text-yellow-500' : 'text-gray-500'
}
</script>

<template>
  <!-- This is where your template goes	-->
   <div id="movies-page">
        <button 
            id="add-movie-button" 
            @click="showAddMovieModal = true"
        >
            Add Movie
        </button>
        <div id="movies-container">
            <div v-for="(movie, movieIndex) in movies" class="movie-card">
                <StarIcon 
                    :class="['star-icon', getStarStyles(movie.rating)]" 
                />
                <p class="star-label">{{ movie.rating ?? '-' }}</p>

                <img :src="movie.image" :alt="`${movie.name} poster`" class="movie-image" />
                <div class="details-container">
                    <h2 class="text-xl font-semibold">{{ movie.name }}</h2>
                    <div class="rating-container">
                        <p class="text-sm">
                            Rating ({{ movie.rating ?? '-' }}/5)
                            <button 
                                v-for="number in 5"
                                @click="onStarClick(number, movieIndex)"
                            >
                                <FontAwesomeIcon 
                                    :style="{color: getFullOrEmptyStarColor(number, movie.rating)}"
                                    :icon="faStarFull"
                                />
                            </button>
                        </p>
                    </div>
                    <div class="genre-container">
                        <div v-for="genre in movie.genres" class="genre-tab">
                            <p>{{ genre }}</p>
                        </div>
                    </div>
                    
                    <p class="text-sm description-text">{{ movie.description }}</p>
                </div>
            </div>
        </div>
        <Transition>
            <AddMovieModal 
                v-if="showAddMovieModal" 
                @close="showAddMovieModal = false"
            />
        </Transition>
   </div>
</template>

<style scoped>
#movies-page {
    max-width: 1200px;
    padding: 20px 30px;
    margin: auto;
    display: flex;
    flex-direction: column;
    gap: 20px;
}

#movies-container {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    grid-gap: 1rem;
}

.movie-card {
    position: relative;
    height: 100%;
    background: white;
    border-radius: 8px;
}

.movie-image {
    height: 400px;
    width: 100%;
}

.details-container {
    padding: 8px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

.rating-container {
    padding: 4px 0px 8px 0;
}

.genre-container {
    display: flex;
    gap: 6px;
    padding-bottom: 8px;
}

.genre-tab {
    background-color: lavender;
    padding: 2px 8px;
    border-radius: 16px;
    font-size: 12px;
}

.star-icon {
    width: 42px;
    height: 42px;
    position: absolute;
    right: 0;
}

.star-label {
    position: absolute;
    right: 0;
    line-height: 42px;
    width: 42px;
    text-align: center;
}

.description-text {
    text-overflow: ellipsis;
    overflow: hidden;
}

#add-movie-button {
    background: rgb(0, 153, 255);
    border-radius: 8px;
    padding: 2px 0px;
    color: white;
}

/* Transition */
.v-enter-active,
.v-leave-active {
  transition: opacity 0.2s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}
</style>
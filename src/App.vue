<template>
  <!-- This is where your template goes	-->
   <div id="movies-page">
        <div class="header">
            <div>
                <p class="text-sm text-white">Total movies: {{ movies.length }}</p>
                <p class="text-sm text-white">Average rating: {{ averageRating }}/5</p>
            </div>
            <button 
                id="add-movie-button" 
                @click="showAddMovieModal = true"
            >
                Add Movie
            </button>
        </div>
        
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

                    <div class="actions">
                        <button class="action-button" @click=editMovie(movieIndex)>
                            <FontAwesomeIcon size="sm" :icon="faEdit" />
                        </button>
                        <button class="action-button" @click="deleteMovie(movieIndex)">
                            <FontAwesomeIcon size="sm" :icon="faTrash" />
                        </button>
                    </div>
                </div>
            </div>
        </div>
        <Transition>
            <AddMovieModal 
                v-if="showAddMovieModal" 
                v-model:name="newMovie.name"
                v-model:image="newMovie.image"
                v-model:description="newMovie.description"
                v-model:selectedGenres="newMovie.genres"
                v-model:isInTheatres="newMovie.inTheaters"
                @addMovie="createMovie"
                @close="resetAndCloseMovieModal"
            />
        </Transition>
   </div>
</template>

<script setup lang="ts">
import { ref, Ref, computed } from "vue"
import { items } from "./movies.json"
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { faStar as faStarFull, faTrash, faEdit } from "@fortawesome/free-solid-svg-icons"
import { StarIcon } from "@heroicons/vue/24/solid"
import AddMovieModal from "./AddMovieModal.vue"

interface Movie {
    id: number | null
    name: string
    description: string
    genres: string[]
    image: string
    inTheaters: boolean
    rating: number | null
}

const movies: Ref<Movie[]> = ref(items)

const showAddMovieModal = ref(false)

const newMovie: Ref<Movie> = ref({
    id: null,
    name: '',
    description: '',
    genres: [],
    image: '',
    inTheaters: false,
    rating: 0
})

const averageRating = computed(() => {
    let totalAverage = 0;
    movies.value.forEach((movie) => {
        totalAverage += movie.rating ?? 0
    })
    return (totalAverage / (movies.value.length ?? 1)).toFixed(1)
})

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

const createMovie = () => {
    if (newMovie.value.id) {
        movies[newMovie.value.id] = newMovie
    } else {
        const createNewMovie = {
        ...newMovie.value,
        id: movies.value.length
    }
    movies.value.push(createNewMovie)
    }

    resetAndCloseMovieModal()
}

const resetAndCloseMovieModal = () => {
    newMovie.value = {
        id: null,
        name: '',
        description: '',
        genres: [],
        image: '',
        inTheaters: false,
        rating: 0
    }
    showAddMovieModal.value = false
}

const editMovie = (movieId: number) => {
    const existingMovie = movies.value.at(movieId)
    if (existingMovie) {
        newMovie.value = existingMovie
    }
    showAddMovieModal.value = true
}

const deleteMovie = (movieId: number) => {
    movies.value.splice(movieId, 1)
}
</script>

<style scoped>
.header {
    display: flex;
    justify-content: space-between;
}

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
    max-width: 320px;
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
    height: calc(100% - 400px);
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
    padding: 2px 12px;
    color: white;
    width: fit-content;
}

.actions {
    display: flex;
    gap: 10px;
    padding-top: 10px;
    justify-content: right;
}

.action-button {
    display: flex;
    justify-content: center;
    align-items: center;
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
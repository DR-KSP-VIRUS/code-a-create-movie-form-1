<template>
    <div class="movie-list">
        <div class="movie-item my-4" v-for="(movie, movieIndex) in movies" :key="movie.id">
            <Movie :movieIndex="movieIndex" :movie="movie" @updateRating="onUpdateStar" @removeMovie="onDeleteMovie"
                @update="onMovieUpdate" />
        </div>
    </div>
</template>

<script setup>
import Movie from './Movie.vue';

const props = defineProps({
    movies: {
        type: Array,
        required: true
    }
});

const emits = defineEmits(['updateStar', 'deleteMovie','movieUpdate']);


const onDeleteMovie = (id) => {
    emits('deleteMovie', id);
}

const onUpdateStar = (index, rating) => {
    emits('updateStar', index, rating);
}

const onMovieUpdate = (index,movieUpdateData) => {
    emits('movieUpdate',index, movieUpdateData);
}
</script>
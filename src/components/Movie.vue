<template>
    <div class="movie-item-image-wrapper">
        <div class="movie-item-star-wrapper">
            <StarIcon id="rating" class="movie-item-star-rating-icon"
                :class="[movie.rating ? 'text-yellow-500' : 'text-gray-500']" />
            <div class="movie-item-star-content-wrapper">
                <span v-if="movie.rating" id="rating-stars" class="movie-item-star-content-rating-rated">
                    {{ movie.rating }}
                </span>
                <span v-else class="movie-item-star-content-rating-not-rated">
                    -
                </span>
            </div>
        </div>
        <img :src="movie.image" class="movie-item-image" alt="" />
    </div>

    <div class="movie-item-content-wrapper">
        <div class="movie-item-title-wrapper">
            <h3 class="movie-item-title">{{ movie.name }}</h3>
            <div class="movie-item-genres-wrapper">
                <span v-for="genre in movie.genres" :key="`${movie.id}-${genre}`" class="movie-item-genre-tag">{{ genre
                    }}</span>
            </div>
        </div>
        <div class="movie-item-description-wrapper">
            <p class="movie-item-description">{{ movie.description }}</p>
        </div>
        <div class="flex place-content-between w-full">
            <div class="movie-item-rating-wrapper">
                <span class="movie-item-rating-text">
                    Rating: ({{ movie.rating }}/5)
                </span>

                <div class="movie-item-star-icon-wrapper">
                    <button v-for="star in 5" :key="star" class="movie-item-star-icon-button" :class="[
                        star <= movie.rating ? 'text-yellow-500' : 'text-gray-500',
                    ]" :disabled="star === movie.rating" @click="onUpdateStar(movieIndex, star)">
                        <StarIcon class="movie-item-star-icon" />
                    </button>
                </div>
                <PencilIcon class="movie-edit-item-icon" @click="() => isEdit = true" />
                <TrashIcon class="movie-trash-item-icon" @click="onDeleteMovie" />
            </div>
        </div>
        <Teleport to="body">
            <EditMovieForm :isEdit="isEdit" :movie="movie" @updateMovie="onUpdate" @closeModal="()=>isEdit=false"/>
        </Teleport>
    </div>
</template>

<script setup>
import { ref } from "vue";
import { StarIcon, TrashIcon, PencilIcon } from "@heroicons/vue/24/solid";
import EditMovieForm from './EditMovieForm.vue';

const isEdit = ref(false);
const props = defineProps({
    movie: {
        type: Object,
        required: true
    },
    movieIndex: {
        type: Number
    },
});

const emits = defineEmits(['updateRating', 'removeMovie', 'update']);



const onUpdateStar = (index, rating) => {
    emits('updateRating', index, rating);
}

const onDeleteMovie = () => {
    emits('removeMovie', props.movie.id);
}

const onUpdate = (updateMovie) => {
    emits('update', props.movieIndex,updateMovie);
}
</script>
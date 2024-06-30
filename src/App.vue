<script setup>
import { computed, ref } from "vue";
import { items } from "./movies.json";
import AddMovie from './components/AddMovie.vue';
import MovieList from './components/MovieList.vue';
const movies = ref(items);
const isOpen = ref(false);

function updateRating(movieIndex, rating) {
  movies.value[movieIndex].rating = rating;
}

const handleAddMovie = (movie) => {
  movies.value.push({ id: Math.floor(Math.random() * 0.4 + 54), ...movie });
}

const handleDelete = (id) => {
  movies.value = movies.value.filter(m=>m.id !== id)
}

const totalMovies = computed(() => movies.value.length);

const averageRatings = computed(() => {
  let average = movies.value.reduce((acc, movie) => (acc + movie.rating), 0) / totalMovies.value || 0;
  return average.toFixed(2);
});

const removeRatings = () => {
  movies.value.forEach(element => {
    element.rating = 0
  });
}

const handleMovieUpdate = (index, upm) => {
  movies.value[index] = upm;
}
</script>

<template>
  <div class="app">
    <Teleport to="body">
      <AddMovie :isOpen="isOpen" @closeModal="() => isOpen = false" @addmovie="handleAddMovie" />
    </Teleport>
    <div class="flex place-items-center place-content-between w-full">
      <div class="ml-4 text-xs md:text-base">
        Total Movies {{ totalMovies }} / Average Ratings {{ averageRatings }}
      </div>
      <div class="mr-6 text-xs md:text-base">
        <button class="bg-blue-700 text-white px-4 py-2 my-4 rounded transition active:scale-90"
          @click="removeRatings">Remove Rating</button>
        <button class="bg-blue-700 text-white px-4 py-2 my-4 rounded transition ml-2 active:scale-90"
          @click="()=>isOpen=true">Add
          Movie</button>
      </div>
    </div>

    <MovieList :movies="movies" @updateStar="updateRating" @deleteMovie="handleDelete" @movieUpdate="handleMovieUpdate"/>
  </div>
</template>



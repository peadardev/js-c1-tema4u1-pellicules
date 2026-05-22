<script setup lang="ts">
import { ref, computed } from 'vue';
import type { Movie } from '@/models/movie.interface';
import moviesImported from '@/data/movies.json';

const isShowAll = ref(true);
const movies = computed<Movie[]>(() =>
  moviesImported.filter((mov) => isShowAll.value || mov.inTheaters)
);
const toggleView = () => (isShowAll.value = !isShowAll.value);
</script>

<template>
  <div>
    <span class="container-control">
      <button @click="toggleView" class="button-toggle">
        {{ isShowAll ? 'Als cinemes' : 'Mostra totes' }}
      </button>
      <div class="number-movies">Mostrant {{ movies.length }} pel·lícules</div>
    </span>
    <span class="movies-list">
      <div class="movie-container" v-for="movie of movies" :key="movie.id">
        <div v-if="isShowAll || movie.inTheaters" class="movie-item">
          <h3>{{ movie.name }} ({{ movie.year }})</h3>
          <img v-bind:src="movie.image" />
          <p>{{ movie.description }}</p>
          <p><strong>Valoració: </strong>{{ movie.rating }}/5</p>
          <span class="movie-end-line">
            <p>{{ movie.genres.reduce((txt, g) => `${txt}, ${g}`) }}</p>
            <p v-if="movie.inTheaters" class="label-in-theaters">Als cinemes</p>
          </span>
        </div>
      </div>
    </span>
  </div>
</template>

<style scoped>
.container-control {
  display: flex;
  justify-content: space-between;
}
.number-movies {
  font-family: Arial, sans-serif;
  font-weight: bold;
  padding: 1rem;
}
img {
  max-width: 100px;
  width: 100%;
  height: auto;
}
.button-toggle {
  padding: 10px 20px;
  margin: 1rem;
  background: lightsteelblue;
  border-radius: 8px;
  cursor: pointer;
  border: 1px solid gray;
}
.movies-list {
  display: flex;
}

.movie-container {
  display: flex;
}
.movie-item {
  max-width: 260px;
  min-width: 200px;
  margin: 20px;
  background: black;
  border: 0px solid black;
  padding: 5px;
  margin: 5px;
  text-align: left;
  background-color: lightyellow;
}
.movie-end-line {
  display: flex;
  justify-content: space-between;
  font-style: italic;
}
.label-in-theaters {
  font-size: 12px;
  font-style: normal;
  color: yellow;
  background: green;
  padding: 8px;
  border-radius: 4px;
}
</style>

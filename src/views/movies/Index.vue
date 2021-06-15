<template>
  <div class="movies-index">
    <div>
      <p>Search</p>
      <input type="text" v-model="titleFilter" placeholder="Search Titles" />
    </div>
    <div
      v-for="movie in filterBy(movies, titleFilter, 'title')"
      v-bind:key="movie.id"
    ></div>
    <div v-for="movie in movies" v-bind:key="movie.id">
      <h2>{{ movie.title }}</h2>
      <p>Year: {{ movie.year }}</p>
      <p>Plot: {{ movie.plot }}</p>
      <router-link :to="`/movies/${movie.id}`">Go to Movie</router-link>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function () {
    return {
      movies: [],
      titleFilter: "",
    };
  },
  created: function () {
    axios.get("/movies").then((response) => {
      console.log("Movies Array", response.data);
      this.movies = response.data;
    });
  },
};
</script>

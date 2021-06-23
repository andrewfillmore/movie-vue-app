<template>
  <div class="movies-index">
    <datalist id="titles">
      <option v-for="movie in movies" v-bind:key="movie.id">
        {{ movie.title }}
      </option>
    </datalist>
    <div class="row g-3 align-items-center">
      <div class="col-auto">
        <label for="inputPassword6" class="col-form-label">Search</label>
      </div>
      <div class="col-auto">
        <input
          type="text"
          id="inputPassword6"
          class="form-control"
          aria-describedby="searchBox"
          v-model="searchTerm"
          list="titles"
        />
      </div>
      <div class="col-auto">
        <span id="searchBox" class="form-text">
          Search for a movie by title.
        </span>
      </div>
    </div>
    <br />
    <button v-on:click="setSortAttribute('title')" class="btn btn-success">
      Sort by title
      <span v-if="sortAttribute === 'title' && sortOrder === 1">^</span>
      <span v-if="sortAttribute === 'title' && sortOrder === -1">v</span>
    </button>
    <div class="row row-cols-1 row-cols-md-3 g-4">
      <div
        class="col"
        v-for="movie in filterBy(
          orderBy(movies, sortAttribute, sortOrder),
          searchTerm
        )"
        v-bind:key="movie.id"
      >
        <div class="card">
          <!-- <img :src="movie.image_url" class="card-img-top" :alt="movie.title" /> -->

          <div class="card-body">
            <h5 class="card-title">{{ movie.title }}</h5>
            <p class="card-text">Plot: {{ movie.plot }}</p>
            <p>Year: {{ movie.year }}</p>
          </div>
          <router-link :to="`/movies/${movie.id}`">Go to Movie</router-link>
        </div>
      </div>
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
      searchTerm: "",
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

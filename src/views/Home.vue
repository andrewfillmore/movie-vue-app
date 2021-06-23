<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2>New Movie</h2>
    Title: <input type="text" v-model="newMovieParams.Title" /><br />
    Year: <input type="text" v-model="newMovieParams.Year" /><br />
    Plot: <input type="text" v-model="newMovieParams.Plot" /><br />
    <button v-on:click="createMovie()">Add Movie</button>
    <form>
      <div>
        <button type="submit">Submit</button>
      </div>
    </form>
    <div v-for="movie in movies" :key="movie.id">
      <h3>Title: {{ movie.title }}</h3>
      <img :src="movie.image_url" alt="" /><br />
      <button v-on:click="showMovie(movie)">More Info</button>
      <p>Year: {{ movie.year }}</p>
      <p>Plot: {{ movie.plot }}</p>
    </div>

    <dialog id="movie-details">
      <form method="dialog">
        <h1>Movie Info</h1>
        <img src="" alt="" />
        <p>Title: ...</p>
        <p>Year: ...</p>
        <p>Plot: ...</p>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Movies Vue App!",
      movies: [],
      newMovieParams: {},
      currentMovie: {},
      errors: [],
    };
  },
  created: function () {
    this.indexMovies();
  },
  methods: {
    indexMovies: function () {
      axios.get("http://localhost:3000/movies").then((response) => {
        console.log(response.data);
        this.movies = response.data;
      });
    },
    createMovie: function () {
      axios
        .post("/movies", this.newMovieParams)
        .then((response) => {
          console.log("Success!", response.data);
          this.movies.push(response.data);
        })
        .catch((error) => {
          console.log(error.response.data.errors);
          this.errors = error.response.data.errors;
        });
    },

    showMovie: function (movie) {
      console.log(movie);
      this.currentMovie = movie;
      document.querySelector("#movie-details").showModal();
    },
  },
};
</script>

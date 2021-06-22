<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <!-- <h2>New Movie</h2>
    Title: <input type="text" v-model="newMovieTitle" /><br />
    Year: <input type="text" v-model="newMovieYear" /><br />
    Plot: <input type="text" v-model="newMoviePlot" /><br />
    <button v-on:click="createMovie()">Add Movie</button> -->
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

    showMovie: function (movie) {
      console.log(movie);
      document.querySelector("#movie-details").showModal();
    },
  },
};
</script>

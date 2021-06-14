<template>
  <div class="movies-new">
    <form v-on:submit.prevent="createMovie()">
      <h1>New Movie</h1>
      <ul>
        <li class="text-danger" v-for="error in errors" v-bind:key="error">
          {{ error }}
        </li>
      </ul>
      <!-- <div>
        Title: <input type="text" v-model="newMovieTitle" /><br />
        Year: <input type="text" v-model="newMovieYear" /><br />
        Plot: <input type="text" v-model="newMoviePlot" /><br />
        
      </div> -->
      <div class="form-group">
        <label>Title:</label>
        <input
          type="text"
          class="form-control"
          v-model="newMovieParams.title"
        />
      </div>

      <div class="form-group">
        <label>Year:</label>
        <input type="text" class="form-control" v-model="newMovieParams.year" />
      </div>
      <div class="form-group">
        <label>Plot:</label>
        <input type="text" class="form-control" v-model="newMovieParams.plot" />
      </div>
      <button v-on:click="createMovie()">Add Movie</button>
      <input type="submit" class="btn btn-primary" value="Submit" />
    </form>
    newMovieParams: {{ newMovieParams }}
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      newMovieParams: {},
      errors: [],
      newMovieTitle: "",
      newMovieYear: "",
      newMoviePlot: "",
    };
  },

  methods: {
    createMovie: function () {
      var params = {
        title: this.newMovieTitle,
        year: this.newMovieYear,
        plot: this.newMoviePlot,
      };
      axios
        .post("http://localhost:3000/movies", params)
        .then((response) => {
          console.log("Success!", response.data);
          this.movies.push(response.data);
        })
        .catch((error) => {
          console.log(error.response.data.errors);
        });
      this.newMovieTitle = "";
      this.newMovieYear = "";
      this.newMoviePlot = "";
    },
  },
};
</script>

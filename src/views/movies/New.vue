<template>
  <div class="movies-new">
    <form v-on:submit.prevent="createMovie()">
      <h1>New Movie</h1>
      <ul>
        <li class="text-danger" v-for="error in errors" v-bind:key="error">
          {{ error }}
        </li>
      </ul>

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
      <small {{ 250 - newMovieParams.plot.length }}>
        characters remaining
      </small>
      <small
        v-if="newPostParams.body.length > 0 && newPostParams.body.length > 200"
        type="text-danger"
        >Characters exceeding limit</small
      >
      <button v-on:click="createMovie()">Add Movie</button>
      <input type="submit" class="btn btn-primary" value="Submit" />
    </form>
    newMovieParams: {{ newMovieParams }}
  </div>
</template>

<style scoped>
text-danger {
  color: red;
}
</style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      newMovieParams: { body: "" },
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

<template>
  <div class="movies-edit">
    <form v-on:submit.prevent="updateMovie()">
      <h1>Edit Movie</h1>
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
          v-model="editMovieParams.title"
        />
      </div>

      <div class="form-group">
        <label>Year:</label>
        <input
          type="text"
          class="form-control"
          v-model="editMovieParams.year"
        />
      </div>
      <div class="form-group">
        <label>Plot:</label>
        <input
          type="text"
          class="form-control"
          v-model="editMovieParams.plot"
        />
      </div>
      <br />

      <input type="submit" class="btn btn-primary" value="Submit" /><br />
      <button v-on:click="destroyMovie()">Delete Movie</button>
    </form>
    editMovieParams: {{ editMovieParams }}
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      editMovieParams: {},
      errors: [],
    };
  },
  created: function () {
    axios.get(`/movies/${this.$route.params.id}`).then((response) => {
      console.log("Movie object", response.data);
      this.editMovieParams = response.data;
    });
  },
  methods: {
    updateMovie: function () {
      axios
        .patch(`/movies/${this.editMovieParams.id}`, this.editMovieParams)
        .then((response) => {
          console.log(response.data);
          this.$router.push(`/movies/${response.data.id}`);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
    destroyMovie: function () {
      if (confirm("Would you like to delete this movie?")) {
        axios.delete(`/movies/${this.movie.id}`).then((response) => {
          console.log(response.data);
          this.$router.push("/movies");
        });
      }
    },
  },
};
</script>

<template>
  <div class="login">
    <form v-on:submit.prevent="submit()">
      <h1>Login</h1>
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <div>
        <label>Email:</label>
        <input type="email" v-model="email" />
      </div>
      <div>
        <label>Password:</label>
        <input type="password" v-model="password" />
      </div>
      <input id="image" type="image" width="100" height="30" alt="Login"
        src="https://raw.githubusercontent.com/mdn/learning-area/master/html/forms/image-type-example/login.png">
      <br />
      <p>Please select your preferred contact method:</p>
      <div>
        <input type="radio" id="contactChoice1" name="contact" value="email">
        <label for="contactChoice1">Email</label>

        <input type="radio" id="contactChoice2" name="contact" value="phone">
        <label for="contactChoice2">Phone</label>

        <input type="radio" id="contactChoice3" name="contact" value="mail">
        <label for="contactChoice3">Mail</label>
      </div>
    </form>
  </div>
</template>


<script>
import axios from "axios";

export default {
  data: function () {
    return {
      email: "",
      password: "",
      errors: [],
    };
  },
  methods: {
    submit: function () {
      var params = {
        email: this.email,
        password: this.password,
      };
      axios
        .post("/sessions", params)
        .then((response) => {
          axios.defaults.headers.common["Authorization"] =
            "Bearer " + response.data.jwt;
          localStorage.setItem("jwt", response.data.jwt);
          this.$router.push("/");
        })
        .catch((error) => {
          console.log(error.response);
          this.errors = ["Invalid email or password."];
          this.email = "";
          this.password = "";
        });
    },
  },
};
</script>

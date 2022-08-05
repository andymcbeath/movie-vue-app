<script>
import axios from "axios";
export default {
  data: function () {
    return {
      newMovieParams: { plot: "" },
      errors: [],
      status: "",
    };
  },
  created: function () {},
  methods: {
    createMovie: function () {
      axios
        .post("/movies", this.newMovieParams)
        .then((response) => {
          console.log("movies create", response);
          this.$router.push("/movies");
        })
        .catch((error) => {
          console.log("movies create error", error.response);
          this.errors = error.response.data.errors;
          this.status = error.response.status;
        });
    },
  },
};
</script>

<template>
  <div class="movies-new">
    <img
      v-if="status"
      :src="`https://http.dog/${status}.jpg`"
      width="500"
      alt=""
    />
    <h1>New Movie</h1>
    <form v-on:submit.prevent="createMovie()">
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      Title:
      <input type="text" v-model="newMovieParams.title" />
      Year:
      <input type="text" v-model="newMovieParams.year" />
      Plot:
      <input type="text" v-model="newMovieParams.plot" />
      <small v-if="newMovieParams.plot.length < 140" class="text-danger"
        >Characters remeaining: {{ 140 - newMovieParams.plot.length }}</small
      >
      <input type="submit" value="Create" />
    </form>
  </div>
</template>

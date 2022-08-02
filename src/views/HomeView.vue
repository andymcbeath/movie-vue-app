<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to my movie app!",
      movies: [],
      newMovieParams: {},
      editMovieParams: {},
    };
  },
  created: function () {
    this.indexMovies();
  },
  methods: {
    indexMovies: function () {
      axios.get("http://localhost:3000/movies").then((response) => {
        this.movies = response.data;
        console.log("All Movies: ", this.movies);
      });
    },
    createMovie: function () {
      axios
        .post("http://localhost:3000/movies", this.newMovieParams)
        .then((response) => {
          console.log("movies create", response.data);
          this.movies.push(response.data);
          this.newMovieParams = {};
        })
        .catch((error) => (this.errorMessage = error));
    },
    showMovie: function (movie) {
      console.log(movie);
      document.querySelector("#movie-details").showModal();
      this.currentMovie = movie;
      this.editMovieParams = movie;
    },
    updateMovie: function (movie) {
      axios
        .patch("http://localhost:3000/movies/" + movie.id, this.editMovieParams)
        .then((response) => {
          console.log("movies updated", response.data);
        });
    },
    destroyMovie: function (movie) {
      axios
        .delete("http://localhost:3000/movies/" + movie.id)
        .then((response) => {
          console.log("Success!", response.data);
          var index = this.movies.indexOf(movie);
          this.movies.splice(index, 1);
        });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>Home Page</h1>
    <div>
      Title:
      <input
        type="text"
        v-model="newMovieParams.title"
        id="title"
        placeholder="title"
      />
      Year:
      <input type="text" v-model="newMovieParams.year" />
      Plot:
      <input type="text" v-model="newMovieParams.plot" />
    </div>
    <div>
      <button v-on:click="createMovie()">Create a Movie</button>
    </div>
    <h2>{{ message }}</h2>
    <h3>All Movies</h3>
    <div v-for="movie in movies" v-bind:key="movie.id">
      <h4>{{ movie.title }}</h4>
      <p>Plot: {{ Movie.plot }}</p>
    </div>
    <div>
      <button v-on:click="showMovie(movie)">More info</button>
    </div>
    <dialog id="movie-details">
      <form method="dialog">
        <h6>Movie info</h6>
        <p>Title: {{ currentMovie.title }}</p>
        <p>Year: {{ currentMovie.year }}</p>
        <p>Plot: {{ currentMovie.plot }}</p>
        <p>
          Title:
          <input type="text" v-model="editMovieparams.title" />
        </p>
        <p>
          Year:
          <input type="text" v-model="editMovieparams.year" />
        </p>
        <p>
          Plot:
          <input type="text" v-model="editMovieparams.plot" />
        </p>
        <button v-on:click="updateMovie(currentMovie)">Update</button>
        <button>Close</button>
        <button v-on:click="destroyMovie(currentMovie)">Destroy Movie</button>
      </form>
    </dialog>
  </div>
</template>

<style></style>

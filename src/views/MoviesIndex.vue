<script>
import axios from "axios";

export default {
  data: function () {
    return {
      movies: [],
      currentMovie: {},
      titleFilter: "",
    };
  },
  created: function () {
    this.indexMovies();
  },
  methods: {
    indexMovies: function () {
      axios.get("/movies").then((response) => {
        console.log("movies index", response);
        this.movies = response.data;
      });
    },
    filterMovies: function () {
      return this.movies.filter((movie) => {
        var lowerTitle = movie.title.toLowerCase();
        var lowerTitleFilter = this.titleFilter.toLowerCase();
        return lowerTitle.includes(lowerTitleFilter);
      });
    },
  },
};
</script>

<template>
  <div class="movies-index">
    <h1>All Movies</h1>
    <div v-for="movie in movies" v-bind:key="movie.id">
      <h2>{{ movie.title }}</h2>
      <p>Year: {{ movie.year }}</p>
      <p>Plot: {{ movie.plot }}</p>
      <router-link v-bind:to="`/movies/${movie.id}`">More details</router-link>
    </div>
  </div>
</template>

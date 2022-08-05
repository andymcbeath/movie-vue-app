<script>
export default {
  data: function () {
    return {
      isLoggedIn: false,
      flashMessage: "Test",
    };
  },
  methods: {
    getUserId: function () {
      return localStorage.getItem("user_id");
    },
  },
  watch: {
    $route: function () {
      localStorage.setItem("flashMessage", "Message!");
      // this.isLoggedIn = !!localStorage.jwt;
      this.flashMessage = localStorage.getItem("flashMessage");
      localStorage.removeItem("flashMessage");
    },
  },
};
</script>

<template>
  <nav>
    <router-link to="/">Home</router-link> |
    <router-link to="/movies">All Movies</router-link> |
    <router-link to="/movies/new">New Movie</router-link> |
    <router-link to="/signup">Signup</router-link> |
    <router-link to="/login">Login</router-link> |
    <router-link to="/logout">Logout</router-link>
  </nav>
  <div
    v-if="flashMessage"
    v-on:click="flashMessage = ''"
    class="alert alert-success"
  >
    {{ flashMessage }}
  </div>
  <router-view />
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>

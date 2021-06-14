<template>
  <div class="movies-show">
    <h1>{{ movie.title }}</h1>
    <p> {{ movie.year }}</p>
    <p> {{ movie.plot }}</p>
    <p> {{ movie.director }} </p>
    <p>Added by: {{ movie }} </p>
    <router-link :to="`http://localhost:3000/movies`" > All Movies  </router-link>
    <button v-on:click="destroyMovie()">Delete</button>
    
  </div>
</template>



<script>
import axios from "axios";
export default {
  data: function () {
    return {
      movie: {}
    };
  },
  created: function () {
    axios.get(`http://localhost:3000/movies/${this.$route.params.id}`).then((response) => {
      console.log("Movie object", response.data);
      this.movie = response.data;
    });
  },
  methods: {
    destroyMovie: function () {
      if (confirm("Are you sure you want to delete this movie?")) {
        axios.delete(`http://localhost:3000/movies/${this.movie.id}`).then((response) => {
          console.log(response.data);
          this.$router.push("/movies");
        });
      }
    }
  }
};
</script>
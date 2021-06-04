<template>
  <div class="home">
    <h1>{{ message }}</h1>

    <h2> New Product </h2>
    Title: <input type="text" v-model="newMovieTitle" /> <br />
    Year: <input type="text" v-model="newMovieYear" /> <br />
    Plot: <input type="text" v-model="newMoviePlot" /> <br />
    Director: <input type="text" v-model="newMovieDirector" /> <br />

    <button v-on:click="createMovie()">Create Movie</button>
    


    <div v-for="movie in movies" v-bind:key="movie.id">
    <h3>Title {{ movie.title }} </h3>
    <button v-on:click="showMovie(movie)">More Info</button>
    <p>Year {{movie.year}} </p>
    <p>Plot {{ movie.plot }} </p>
    <p>Director {{ movie.director }}</p>

    <dialog id="movie-details">
      <form method="dialog">
        <h1>Movie Info</h1>
        <img src="" alt="" />
        <p>Title: {{ currentMovie.title }}</p>
        <p>Year: {{ currentMovie.year }}</p>
        <p>Plot: {{ currentMovie.plot }}</p>
        <p>Director: {{ currentMovie.director }}</p>
        <button>Close</button>
      </form>
    </dialog>


    </div>
  </div>
</template>

<style></style>

<script>
  import axios from "axios";
  export default {
    data: function () {
      return {
        message: "Benjamins Favorite Movies!",
        movies: "indexMovie",
        newMovieTitle: "",
        newMovieYear: "",
        newMoviePlot: "",
        newMovieDirector: "",
        currentMovie: ""
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
    createMovie: function () {
      var params = {
        name: this.newMovieTitle,
        description: this.newMovieYear,
        price: this.newMoviePlot,
        director: this.newMovieDirector
      };
      axios.post("http://localhost:3000/movies", params).then((response) => {
        console.log("Success!", response.data);
        this.movies.push(response.data);
      })
      .catch((error) => {
        console.log(error.response.data.errors);
      });
    },
    showMovie: function (movie) {
      console.log(movie);
      this.currentMovie = movie;
      document.querySelector("#movie-details").showModal();
    }
    }
    };
</script>
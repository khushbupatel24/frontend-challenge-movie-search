<template>
  <div id="app" class="container pt-5">
    <div class="row">
      <div class="col-md-12">
        <h2 class="p-2">The Shoppies</h2>
        <searchMovie
          :searchText="searchText"
          @searchUpdated="updateSearchText"
        />
      </div>
    </div>
    <div class="row">
      <div class="col-md-6 pt-4">
        <list-movies
          :searchText="searchText"
          :searchResults="searchResults"
          :nominatedMovies="nominatedMovies"
          :isFetching="isFetching"
          @nominateMovieParent="nominateMovie"
        />
      </div>
      <div class="col-md-6 pt-4">
        <nominies
          :nominatedMovies="nominatedMovies"
          @removeNominationParent="removeNomination"
        />
      </div>
    </div>
  </div>
</template>

<script>
import SearchMovie from "./components/movie/Search.vue";
import ListMovies from "./components/movie/List.vue";
import Nominies from "./components/movie/Nominies.vue";

import axios from "axios";

export default {
  name: "App",
  components: {
    SearchMovie,
    ListMovies,
    Nominies,
  },
  data() {
    return {
      searchText: "",
      searchResults: [],
      nominatedMovies: [],
      isFetching: false,
    };
  },
  created() {
      if(JSON.parse(localStorage.getItem("movies")) != undefined)
        this.nominatedMovies = JSON.parse(localStorage.getItem("movies"))
  },
  methods: {
    updateSearchText(value) {
      if (value == "" || value == null) {
        alert("Please enter something in search text");
      }
      this.searchText = value;
      this.isFetching = true;
      axios
        .get("https://www.omdbapi.com/?apikey=67452df7&type=movie&s=" + value)
        .then((response) => {
          console.log(response);
          this.isFetching = false;
          if (response.data.Response != "False") {
            this.searchResults = response.data.Search;
          }
        });
    },
    nominateMovie(value) {
      if (this.nominatedMovies.length == 5) {
        alert("Opps! You reached the limit of 5 nominations.");
        return false;
      }
      if (!this.nominatedMovies.find((movie) => movie.imdbID === value.imdbID)) {
          this.nominatedMovies.push(value);
          localStorage.setItem("movies", JSON.stringify(this.nominatedMovies))
      }
    },
    removeNomination(imdbID) {
      this.nominatedMovies = this.nominatedMovies.filter(function (obj) {
        return obj.imdbID !== imdbID;
      });
      localStorage.setItem("movies", JSON.stringify(this.nominatedMovies))
    },
  },
};
</script>
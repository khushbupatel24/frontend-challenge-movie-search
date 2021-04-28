<template>
  <div class="card">
    <div class="card-body">
      <p class="card-title">
        <b>Results for "{{ searchText }}"</b>
      </p>
      <div
        v-if="isFetching"
        class="spinner-border text-secondary"
        style="
          width: 3rem;
          height: 3rem;
          position: absolute;
          z-index: 100;
          top: 0;
          left: 0;
          right: 0;
          bottom: 0;
          margin: auto;
        "
        role="status"
      ></div>
      <ul>
        <li
          class="p-2"
          v-for="searchResult in searchResults"
          :key="searchResult.imdbID"
        >
          {{ searchResult.Title }} ( {{ searchResult.Year }} )
          <button
            :disabled="isNominated(searchResult.imdbID)"
            class="btn btn-sm btn-secondary"
            @click="nominateMovie(searchResult)"
          >
            Nominate
          </button>
        </li>
        <li v-if="searchResults.length == 0">No results found</li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "List",
  props: {
    searchResults: Array,
    searchText: String,
    nominatedMovies: Array,
    isFetching: Boolean,
  },
  data() {
    return {
      search: this.searchText,
    };
  },
  methods: {
    isNominated(imdbID) {
      return this.nominatedMovies.find((movie) => movie.imdbID === imdbID);
    },
    nominateMovie(imdbID) {
      this.$emit("nominateMovieParent", imdbID);
    },
  },
};
</script>
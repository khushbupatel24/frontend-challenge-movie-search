<template>
  <div class="card">
    <div class="card-body">
      <p class="card-title"><b>Nominations</b></p>
      <ul>
        <li
          class="p-2"
          v-for="nominatedMovie in nominatedMovies"
          :key="nominatedMovie.imdbID"
        >
          {{ nominatedMovie.Title }} ( {{ nominatedMovie.Year }} )
          <button
            class="btn btn-sm btn-danger"
            @click="removeNomination(nominatedMovie.imdbID)"
          >
            Remove
          </button>
        </li>
        <li v-if="(nominatedMovies.length == 0)">No movies nominated yet</li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "Nominies",
  props: {
    nominatedMovies: Array,
  },
  methods: {
    removeNomination(imdbID) {
      this.$emit("removeNominationParent", imdbID);
    },
  },
};
</script>
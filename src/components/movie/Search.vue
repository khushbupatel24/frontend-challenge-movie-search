<template>
  <div class="card">
    <div class="card-body">
      <p class="card-title"><b>Search movies</b></p>
      <div class="input-group mb-3">
        <div class="input-group-prepend">
          <span class="input-group-text"><i class="fa fa-search"></i></span>
        </div>
        <input
          type="text"
          placeholder="Enter movie title here"
          class="form-control"
          v-model="search"
          autofocus
          @change="updateSearchText"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Search",
  props: {
    searchText: String,
  },
  data() {
    return {
      search: this.searchText,
    };
  },
  watch: {
    search: function () {
    },
  },
  methods: {
    updateSearchText() {
            this.$emit("searchUpdated", this.search);
    },
  },
};
</script>
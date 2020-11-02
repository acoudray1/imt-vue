<template>
  <div id="list-component">
    <div class="header">
      <h1>TV shows</h1>
      <div class="search-wrapper">
        <label>Find a show:</label>
        <input type="text" v-model="search" placeholder="title..." />
      </div>
    </div>
    <hr />
    <div class="wrapper">
      <div v-for="show in filteredList" :key="show.id">
        <b-card
          :title="show.title"
          :img-src="show.images.show"
          img-alt="Image"
          img-top
          tag="article"
          style="max-width: 30rem; background-color: rgb(9, 22, 37); color: #fff; padding: 1% 1% 4% 1%;"
          class="mb-2"
        >
          <b-button :href="'/show/' + show.id" variant="primary"
            >View details</b-button
          >
          <b-button v-on:click="fav(show.id, show.title)" variant="success"
            >Add to favorites</b-button
          >
        </b-card>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "list-component",
  data() {
    return {
      search: "",
      shows: []
    };
  },
  async fetch() {
    this.shows = await this.$http.$get("http://localhost:4000/rest/shows");
  },
  computed: {
    filteredList() {
      return this.shows.filter(show => {
        return show.title.toLowerCase().includes(this.search.toLowerCase());
      });
    }
  },
  methods: {
    fav: function(id, name) {
      alert('Adding "' + name + '" to favorites...');
      this.$http.$post("http://localhost:4000/rest/shows/" + id + "/favorites");
    }
  }
};
</script>

<style scoped>
#list-component {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  width: 100%;
}

.header {
  width: 100%;
  height: 20%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 2% 2% 0% 2%;
  z-index: 30;
}

hr {
  color: black;
  width: 90%;
  border-width: 1px;
  border-style: solid;
  border-color: black;
}

.wrapper {
  display: grid;
  width: 100vw;
  grid-template-columns: 30vw 30vw 30vw;
  align-items: center;
  justify-items: center;
  justify-content: center;
  align-content: center;
}

.show-description {
  width: 30%;
  height: 60%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
}
</style>

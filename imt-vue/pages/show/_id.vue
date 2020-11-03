<template>
  <div>
    <div class="card">
      <div v-if="notEmpty">
        <div class="header">
          <h1>
            <a href="/">
              <b-icon-house-fill
                variant="light"
                font-scale="1"
              ></b-icon-house-fill></a
            >Show Details
          </h1>
        </div>
        <hr />
        <b-img
          center
          :src="serie.images.banner"
          fluid
          alt="Responsive image"
          class="image"
        ></b-img>
        <div class="content">
          <b-embed
            v-if="serie.next_trailer != null"
            type="iframe"
            aspect="16by9"
            :src="'https://www.youtube.com/embed/' + serie.next_trailer"
            class="youtube-embed"
          ></b-embed>
          <h1>{{ serie.title }}</h1>
          <h4>
            <b-badge v-for="item in serie.genres" :key="item">{{
              item
            }}</b-badge>
          </h4>
          <div class="rating">
            <h6>Global Rating</h6>

            <b-form-rating v-model="serie.notes.mean" readonly></b-form-rating>
          </div>
          <p>{{ serie.description }}</p>
          <p>Number of seasons : {{ serie.seasons }}</p>
          <p>Total nuber of episodes : {{ serie.episodes }}</p>
          <h2>
            Seasons details
          </h2>
          <b-table striped hover :items="serie.seasons_details"></b-table>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import { BootstrapVue, IconsPlugin } from "bootstrap-vue";

Vue.use(BootstrapVue);
Vue.use(IconsPlugin);
export default {
  validate({ params }) {
    return /^\d*$/.test(params.id);
  },
  data() {
    return {
      serie: {},
      notEmpty: false
    };
  },
  async fetch() {
    this.serie = await this.$http.$get(
      "http://localhost:4000/rest/shows/" + this.$route.params.id
    );

    this.notEmpty = true;
  }
};
</script>

<style scoped>
.b-icon {
  margin-right: 2%;
}
.rating {
  width: 15%;
  padding: 1rem;
  border-radius: 5px;
  background-color: gold;
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
.badge {
  margin: 0 0.5rem;
}
hr {
  color: black;
  width: 90%;
  border-width: 1px;
  border-style: solid;
  border-color: black;
}
.card {
  background-color: #092237;
  color: white;
}

.card .content {
  margin: 0 10%;
}

.image {
  width: 100%;
  margin-bottom: 2%;
}

.table {
  color: white;
}

/*.youtube-embed {
  width: auto;
  height: 30vh;
}*/
</style>

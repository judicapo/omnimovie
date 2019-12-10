<template>
  <div class="card">
    <div class="card-content">
      <p class="title">
        Trending movies
      </p>
      <p class="subtitle">
        {{ date }}
      </p>
      <b-carousel icon-pack="fas">
        <b-carousel-item v-for="(item, i) in data" :key="i">
          <div
            class="columns container"
            v-bind:style="{
              backgroundImage: `url(${getImgUrl(item.poster_path)})`
            }"
          >
            <img
              class="column is-2 is-offset-2"
              :src="getImgUrl(item.poster_path)"
            />
            <div class="shadow column is-5 is-offset-1">
              <h1 class="title">{{ item.original_title }}</h1>
              <h2>{{ item.overview }}</h2>
            </div>
          </div>
        </b-carousel-item>
      </b-carousel>
    </div>
  </div>
</template>

<script>
export default {
  name: "Carousel",
  props: {
    mainLoadAsyncData: Function
  },
  methods: {
    async loadAsyncData() {
      const params = {
        sortField: this.sortField,
        sortOrder: this.sortOrder,
        page: this.page
      };

      this.loading = true;
      const { data } = await this.mainLoadAsyncData(params);

      this.data = [];
      for (let i = 0; i < 5; i++) {
        if (!data.results[i]) break;
        data.results[i].release_date = data.results[i].release_date.replace(
          /-/g,
          "/"
        );
        this.data.push(data.results[i]);
      }
    },
    getImgUrl(value) {
      return ` http://image.tmdb.org/t/p/w342${value}`;
    }
  },
  data() {
    return {
      data: [],
      sortField: "vote_count",
      sortOrder: "desc",
      defaultSortOrder: "desc",
      page: 1,
      date: new Date().toDateString()
    };
  },
  mounted() {
    this.loadAsyncData();
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  background-repeat: no-repeat;
  background-size: cover;
  padding-bottom: 3em;
  padding-top: 3em;
}

.shadow {
  color: white;
  max-height: 16em;
  background-color: rgba(0, 0, 0, 0.2);
}
</style>

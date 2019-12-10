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
          <div class="container">
            <img class="image" :src="getImgUrl(item.poster_path)"/>
            <img class="image-small" :src="getImgUrl(item.poster_path)"/>
            <div class="centered">
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
        console.log(data.results[i]);
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
  position: relative;
  text-align: center;
  color: white;
  max-height: 35em
}

.image {
  width: 100%;
}

.image-small{
  position: absolute;
  top: 50%;
  left: 20%;
  transform: translate(-50%, -50%);
}

/* Centered text */
.centered {
  position: absolute;
  top: 50%;
  left: 60%;
  background-color: rgba(0, 0, 0, 0.2);
  transform: translate(-50%, -50%);
}
</style>

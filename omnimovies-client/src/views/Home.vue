<template>
  <div class="container is-fluid">
    <div class="notification">
      <Carousel :mainLoadAsyncData="this.loadAsyncData"></Carousel>
      <List :mainLoadAsyncData="this.loadAsyncData"></List>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Carousel from "@/components/home/Carousel.vue";
import List from "@/components/home/List.vue";

import axios from "axios";

export default {
  name: "home",
  components: {
    Carousel,
    List
  },
  data() {
    return {
      data: [],
      page: 0
    };
  },
  methods: {
    /*
     * Load async data
     */
    async loadAsyncData(params) {
      if (this.page === params.page && this.data) return this.data;

      const query = [
        "api_key=bb6f51bef07465653c3e553d6ab161a8",
        "language=en-US",
        "include_adult=false",
        "include_video=false",
        `sort_by=${params.sortField}.${params.sortOrder}`,
        `page=${params.page}`
      ].join("&");

      try {
        this.data = await axios.get(
          `https://api.themoviedb.org/3/discover/movie?${query}`
        );
        this.page = params.page;
        
        return this.data;
      } catch (error) {
        throw error;
      }
    }
  }
};
</script>

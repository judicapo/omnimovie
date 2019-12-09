<template>
  <div class="home">
    <section class="hero">
      <div class="hero-body">
        <div class="container">
          <h1 class="title">
            Trending Movies
          </h1>
        </div>
      </div>
    </section>
    <Carousel></Carousel>
    <List></List>
  </div>
</template>

<script>
// @ is an alias to /src
import Carousel from '@/components/home/Carousel.vue'
import List from '@/components/home/List.vue'

export default {
  name: 'home',
  components: {
    Carousel,
    List
  }
}
</script>

<template>
  <div class="movie-detail">
    <h2>{{ movie.Title }}</h2>
    <p class="year">{{ movie.Year }}</p>
    <img :src="movie.Poster" alt="Poster" class="featured-img" />
    <p class="plot">{{ movie.Plot }}</p>
  </div>
</template>

<script>
import { ref, onBeforeMount } from "vue";
import { useRoute } from "vue-router";
import env from "@/env.js";

export default {
  setup() {
    const movie = ref({});
    const route = useRoute();

    onBeforeMount(() => {
      fetch(
        `http://www.omdbapi.com/?apikey=${env.apikey}&i=${route.params.id}&plot=full`
      )
        .then((response) => response.json())
        .then((data) => {
          movie.value = data;
        });
    });
    return {
      movie,
    };
  },
};
</script>

<style lang="scss">
$medium: 900px;

.movie-detail {
  padding: 16px;

  h2 {
    color: #fff;
    font-size: 28px;
    font-weight: 600;
    margin-bottom: 16px;
  }

  .featured-img {
    display: block;
    max-width: 200px;
    margin-bottom: 16px;
  }

  p {
    color: #fff;
    line-height: 1.4;
    font-size: 18px;
  }
}

@media screen and (min-width: $medium) {
  .movie-detail {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;

    h2 {
      color: #fff;
      font-size: 28px;
      font-weight: 600;
      margin: 16px 0px;
    }

    .year {
      margin-bottom: 10px;
      font-size: 25px;
    }

    .featured-img {
      display: block;
      max-width: 600px;
      margin-bottom: 16px;
    }

    .plot {
      margin: 50px;
      font-size: 20px;
    }
  }
}
</style>

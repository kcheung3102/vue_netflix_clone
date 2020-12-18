<template>
  <div class="ma-0">
    <header class="banner" :style="headerStyle">
      <div class="banner__contents">
        <h1 class="banner__title">{{ bannerTitle }}</h1>
        <div class="banner__buttons">
          <button class="banner__button rounded-0">Play</button>
          <button class="banner__button rounded-0">My List</button>
        </div>
        <p class="banner__description">{{ description }}</p>
      </div>
      <div class="banner__fadeBottom" />
    </header>
  </div>
</template>

<script>
import axios from "axios";
import requests from "../requests";

export default {
  data() {
    return {
        showLoading = true,
        movie: {},
        size: "cover",
        position: "center center",
        image: "",
    };
  },
  asyc mounted () {
      this.showLoading = true;
      try {
          const response = await axios.get(requests.fetchNetflixOriginals);
          const { results } = response.data;
          const movieIndex = Math.floor(Math.random() * results.length - 1);
          this.movie = results[movieIndex]
          this.results =[movieIndex]?.backdrop_path;
          console.log(this.image);
      } catch (error) {
          console.error(error);
      } finally {
          this.showLoading = false;
      }
  },
  computed: {
      //shortens the description overiew to 150 characters
     description() {
          let n = 150;
          const movieDescription = this.movie?.overview;
          return movieDescription?.length > n
            ? movieDescription.substr(0, n - 1) + "..."
            : movieDescription
      },
      bannerTitle: function () {
          return this.movie?.title || this.movie?.name || this.movie?.original_name;
      }
  },
 
};
</script>

<style scoped>
.banner {
  color: white;
  object-fit: contain;
  height: 448px;
  width: 100%;
}
.banner__contents {
  margin-left: 30px;
  padding-top: 100px;
  height: 190px;
}
.banner__title {
  font-size: 3rem;
  font-weight: 800;
  padding-bottom: 0.3rem;
}
.banner__description {
  width: 45rem;
  margin-top: 1rem;
  line-height: 1.5;
  padding-top: 1rem;
  font-size: 0.9rem;
  max-width: 360px;
  height: 80px;
}
.banner__button {
  cursor: pointer;
  color: white;
  outline: none;
  border: none;
  font-weight: 700;
  border-radius: 0.2vw;
  padding-left: 2rem;
  padding-right: 2rem;
  margin-right: 1rem;
  padding-top: 0.5rem;
  background-color: rgb(51, 51, 51, 0.5);
  padding-bottom: 0.5rem;
}
.banner__button:hover {
  color: #000;
  background-color: #e6e6e6;
  transition: all 0.2s;
}
</style>

<script setup>
import { ref, watch } from "vue";

const URL = import.meta.env.VITE_APP_DATA_URL;
const KEY = import.meta.env.VITE_APP_DATA_KEY;

const input = ref("");
let data = ref(null);

async function getData() {
  const res = await fetch(`${URL}?apikey=${KEY}&s=${input.value}`);
  const dataRes = await res.json();
  if (res.ok) {
    console.log("Data fetched sucssefully!");
  }
  data.value = await dataRes.Search;
}

watch(input, () => {
  input.value.length >= 3 ? getData() : " ";
});
</script>

<template>
  <main>
    <div class="header">
      <h2>Look for your movie!</h2>
      <input type="text" v-model="input" />
    </div>
    <div class="wrapper">
      <div class="movie-wrap" v-for="item in data">
        <img :src="item.Poster" :alt="item.Title" />
        <span class="title"
          >{{ item.Title }} <span class="year">[{{ item.Year }}]</span></span
        >
      </div>
    </div>
  </main>
</template>

<style>
#app {
  position: relative;
  width: 100%;
  height: 100%;
  max-width: 1000px;
  margin: 0 auto;
  background-color: rgb(17, 17, 17);
  padding: 40px;
}

.wrapper {
  display: flex;
  flex-wrap: wrap;
}

.header {
  text-align: center;
}

.movie-wrap {
  position: relative;
  width: 33.33%;
  padding: 20px;
  transition: all 0.3s ease-in-out;
  font-size: 0.8rem;
}

.movie-wrap img {
  position: relative;
  width: 100%;
  border-radius: 14px;
}

.movie-wrap .title {
  display: block;
  margin-top: 10px;
  gap: 10px;
}

.movie-wrap .year {
  font-size: 0.6rem;
  opacity: 0.4;
}

.movie-wrap:hover {
  transform: scale(1.02);
}
</style>

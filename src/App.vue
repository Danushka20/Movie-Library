<script setup>
import Header from './components/Header.vue';
import Hero from './components/Hero.vue';
import Introduction from './components/Introduction.vue';
import MovieGrid from './components/MovieGrid.vue';
import ContactMap from './components/ContactMap.vue';
import Footer from './components/Footer.vue';
import './assets/main.css';

</script>

<template>
  <div id="app">
    <Header/>
    <Hero/>
    <Introduction/>
    <MovieGrid/>
    <ContactMap/>
    <Footer/>
  </div>
</template>

<style>
#app {
  font-family: var(--main-font);
}

</style>
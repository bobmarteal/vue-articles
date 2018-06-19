<template>
  <div id="app">
    <!-- Create simple card listing of A&S news articles -->
    <app-article-cards></app-article-cards>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld'

import articleCards from './components/articleCards.vue';


export default {
  name: 'App',
  components: {
    appArticleCards: articleCards
  }
}
</script>

<style>

</style>

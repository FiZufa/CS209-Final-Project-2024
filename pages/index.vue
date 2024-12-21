<template>
    <div id="app">
      <AppHeader />
      <AppBanner />
      <router-view /> <!-- This renders the page content based on the current route -->
    </div>
  </template>
  
  <script>
  import AppHeader from '@/components/AppHeader.vue';
  import AppBanner from '@/components/AppBanner.vue';
  import Footer from '@/components/Footer.vue';
  
  export default {
    name: 'App',
    components: {
      AppHeader,
      AppBanner,
      Footer,
    }
  }
  </script>
  
  <style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
  
  body {
    margin: 0;
  }
  
  </style>
  
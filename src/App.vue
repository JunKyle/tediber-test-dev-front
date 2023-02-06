<template>
  <div id="app">
    <Header />
    <Home :code="'sofa-simple'"/>
    <Footer />
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'
import Home from './pages/Home.vue'

export default {
  name: 'App',
  components: {
    Home,
    Header,
    Footer
  }
}
</script>

<style>
html {
  font-family: Arial, sans-serif;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
}

body, html {
  height: 100%;
  background-color: #FFFFFF;
  margin: 0;
  overflow: auto;
}

#app {
  position: relative;
  height: auto;
}
</style>

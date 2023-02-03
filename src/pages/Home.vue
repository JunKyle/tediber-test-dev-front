<template>
  <main class="Home">
  </main>
</template>

<script>
export default {
  name: 'Home',
  mounted () {
    this.getHomeData ();
  },
  methods: {
    getHomeData () {
       this.$http
        .get('./data/api.json')
        .then(response => {
          console.log('response data :', response.data);
        });
    } 
  }
}
</script>

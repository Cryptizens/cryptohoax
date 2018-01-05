<template>
  <div id="app">
      <div class="container">
        <app-title></app-title>
        <br>
        <app-quote></app-quote>
        <app-button></app-button>
      </div>
      <app-footer></app-footer>
  </div>
</template>

<script>
import Title from './components/Title.vue';
import Quote from './components/Quote.vue';
import Button from './components/Button.vue';
import Footer from './components/Footer.vue';

export default {
  name: 'app',
  components: {
    'app-title': Title,
    'app-quote': Quote,
    'app-button': Button,
    'app-footer': Footer
  }
}
</script>

<style lang="scss">
body {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(20deg,rgb(219,112,147),#daa357);
  color: white;
}

#app {
  display: flex;
  min-height: calc(100vh - 50px);
  flex-direction: column;
}

.container {
  padding-top: 10%;
  flex: 1;
}
</style>

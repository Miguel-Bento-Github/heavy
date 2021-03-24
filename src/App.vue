<template>
  <Button msg="Start" />
</template>

<script>
  import Button from './components/Button.vue';

  export default {
    name: 'App',
    components: {
      Button,
    },
  };
</script>

<style>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  #app {
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    background: conic-gradient(from 0.5turn at bottom center, #add8e6, #ffffff);
    height: 100vh;
    width: 100%;
    padding: 5rem;
  }

  @import url('https://fonts.googleapis.com/css?family=Pontano+Sans:400');

  html {
    font-size: 100%;
  } /*16px*/

  body {
    font-family: 'Pontano Sans', sans-serif;
    font-weight: 400;
    line-height: 1.75;
  }

  p {
    margin-bottom: 1rem;
  }

  h1,
  h2,
  h3,
  h4,
  h5 {
    margin: 3rem 0 1.38rem;
    font-family: 'Pontano Sans', sans-serif;
    font-weight: 400;
    line-height: 1.3;
  }

  h1 {
    margin-top: 0;
    font-size: 3.052rem;
  }

  h2 {
    font-size: 2.441rem;
  }

  h3 {
    font-size: 1.953rem;
  }

  h4 {
    font-size: 1.563rem;
  }

  h5 {
    font-size: 1.25rem;
  }

  small,
  .text_small {
    font-size: 0.8rem;
  }
</style>

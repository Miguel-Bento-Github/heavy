<template>
  <main @resize="setRect" @mousemove="moveApp" ref="app" class="app">
    <Button @split="showSplit = !showSplit" />
    <Split v-show="showSplit" />
  </main>
</template>

<script>
  import Button from './components/Buttons.vue';
  import Split from './components/Split.vue';

  export default {
    name: 'App',
    components: {
      Button,
      Split,
    },
    data() {
      return {
        rect: 0,
        showSplit: false,
      };
    },
    methods: {
      a() {
        console.log('a');
      },
      moveApp({ clientX, clientY }) {
        const { app } = this.$refs;
        const { left, right, top, bottom } = this.rect;

        const horizontalCenter = (left + right) / 2;
        const x = horizontalCenter - clientX;

        const verticalCenter = (top + bottom) / 2;
        const y = verticalCenter - clientY;

        app.style.setProperty('--shadowX', `${x / 100}px`);
        app.style.setProperty('--shadowY', `${-y / 50}px`);
        app.style.setProperty('--rotateY', `${x / 100}deg`);
        app.style.setProperty('--rotateX', `${-y / 50}deg`);
      },
      setRect() {
        const { app } = this.$refs;
        this.rect = app.getBoundingClientRect();
      },
    },
    mounted() {
      this.setRect();
    },
  };
</script>

<style lang="scss">
  @import url('https://fonts.googleapis.com/css?family=Pontano+Sans:400');
  @import './animation/morph.css';
  @import './css/typography.css';

  :root {
    --default: #ace8f1;
    --light: #c7f8ff;
    --dark: #93b8c4;
    --text: #2c3e50;
  }

  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  #app {
    font-family: 'Pontano Sans', sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: var(--text);
    height: 100vh;
    width: 100vw;
    padding: 10rem;
  }

  .app {
    border-radius: 1rem;
    box-shadow: calc(-1 * var(--shadowX)) var(--shadowY) 30px var(--light),
      var(--shadowX) calc(-1 * var(--shadowY)) 30px var(--dark);
    padding: 5rem;
    width: 90%;
    transition: all 0.4s ease;
  }

  .app:hover {
    transform: rotateY(var(--rotateY)) rotateX(var(--rotateX));
  }

  .fade-enter-active,
  .fade-leave-active {
    transition: opacity 0.4s ease-in-out;
  }
  .fade-enter,
  .fade-leave-to {
    opacity: 0;
  }
</style>

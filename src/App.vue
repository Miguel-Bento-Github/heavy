<template>
  <main class="main">
    <section
      @resize="setRect"
      @mousemove="moveApp"
      ref="app"
      class="app"
      :class="`app--${activeTheme}`"
    >
      <Button @split="showSplit = !showSplit" @update-theme="updateTheme()" />
      <Split
        :isLoading="isLoading"
        :url="imageURL"
        :isOpen="showSplit"
        @update-image="updateImage()"
      />
    </section>
    <transition name="fade">
      <h2 v-if="!showSplit">
        Learn more about mix blend mode through Mozilla's
        <a href="https://developer.mozilla.org/en-US/docs/Web/CSS/mix-blend-mode">mix-blend-mode</a>
        documentation
      </h2>
    </transition>
  </main>
</template>

<script>
  import Button from './components/Buttons.vue';
  import Split from './components/Split.vue';
  import theme from './assets/themes.json';
  import { createApi } from 'unsplash-js';

  export default {
    name: 'App',
    components: {
      Button,
      Split,
    },
    data() {
      return {
        isLoading: false,
        cardSize: null,
        showSplit: false,
        theme: theme,
        activeTheme: 'light',
        imageURL: `https://source.unsplash.com/random/`,
        imageAPI: null,
      };
    },
    methods: {
      async setImageAPI() {
        this.imageAPI = createApi({
          apiUrl: 'http://192.168.5.213:8080/',
        });
      },
      updateTheme() {
        const { passion, light } = this.theme;
        const newTheme = this.activeTheme === 'light' ? passion : light;
        this.updateColorProperties(Object.entries(newTheme));
        this.updateImage();
        this.activeTheme = this.activeTheme === 'light' ? 'passion' : 'light';
      },
      async updateImage() {
        this.isLoading = true;
        const randomChar = Math.random()
          .toString(36)
          .substring(10);

        const img = await fetch(this.imageURL);

        if (img.url.includes('404')) {
          this.updateImage();
        } else {
          this.imageURL = `${this.imageURL}?${randomChar}`;
        }

        setTimeout(() => {
          this.isLoading = false;
        }, 1300);
      },
      /**
       * Sets the new theme colors by changing the css property correspondent to @arg name
       *
       * Currently supports any theme by passing the correct array.
       * Please refer to /assets/themes.json for a schema.
       *
       * @param {Array<Array<string>>} colors
       * @returns {void}
       */
      updateColorProperties(colors) {
        colors.forEach(([name, hex]) => {
          document.body.style.setProperty(`--${name}`, hex);
        });
      },
      moveApp(event) {
        this.setClientCoordinates(event);
      },
      /**
       * Sets card size.
       */
      setRect() {
        const { app } = this.$refs;
        this.cardSize = app.getBoundingClientRect();
      },
      setClientCoordinates(event) {
        const { clientX, clientY } = event;
        const { app } = this.$refs;
        const { left, right, top, bottom } = this.cardSize;

        const horizontalCenter = (left + right) / 2;
        const x = horizontalCenter - clientX;

        const verticalCenter = (top + bottom) / 2;
        const y = verticalCenter - clientY;

        app.style.setProperty('--shadowX', `${x / 100}px`);
        app.style.setProperty('--shadowY', `${-y / 100}px`); // I use the negative values to introduce the idea of mouse weight.
        app.style.setProperty('--rotateY', `${x / 100}deg`);
        app.style.setProperty('--rotateX', `${-y / 100}deg`); // I use the negative values to introduce the idea of mouse weight.
      },
    },
    mounted() {
      this.setRect();
      this.setImageAPI();
    },
  };
</script>

<style lang="scss">
  @import url('https://fonts.googleapis.com/css?family=Pontano+Sans:400');
  @import './animation/morph.css';
  @import './css/typography.css';

  :root {
    --default: rgb(185, 234, 241);
    --light: rgb(199, 248, 255);
    --dark: rgb(146, 183, 195);
    --text: rgb(44, 62, 80);
    --shadowX: 5px;
    --shadowY: -5px;
  }

  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  html,
  body {
    height: 100%;
    width: 100%;
  }

  p {
    isolation: isolate;
  }

  a {
    display: inline-block;
    position: relative;
    color: var(--text);
    text-decoration: none;
    padding: 0 0.4rem;

    &::after {
      position: absolute;
      content: '';
      top: 0;
      bottom: 0;
      right: 0;
      left: 0;
      background: currentColor;
      mix-blend-mode: exclusion;
      transform: scaleY(0.05);
      transform-origin: 0 95%;
      transition: transform 0.2s;

      border-radius: 4px;
    }

    &:focus {
      outline: none;
    }
    &:focus,
    &:hover {
      &::after {
        transform: none;
      }
    }
  }

  #app {
    font-family: 'Pontano Sans', sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: var(--text);
    height: 100%;
    width: 100%;
    display: grid;
    overflow: hidden;
  }

  .app {
    border-radius: 1rem;
    box-shadow: calc(-1 * var(--shadowX)) var(--shadowY) 30px var(--default),
      var(--shadowX) calc(-1 * var(--shadowY)) 30px var(--text);
    padding: 5rem;
    transition: box-shadow 0.4s ease, transform 0.2s ease-out;
  }

  .app:hover {
    transform: rotateY(var(--rotateY)) rotateX(var(--rotateX));
  }

  .main {
    padding: 5rem;
  }

  .fade-enter-active,
  .fade-leave-active {
    transition: opacity 1.2s ease-out;
  }

  .fade-enter-from,
  .fade-leave-to {
    opacity: 0;
  }
</style>

<template>
  <main class="main">
    <section
      @resize="setRect"
      @mousemove="moveApp"
      ref="app"
      class="app"
      :class="`app--${activeTheme}`"
    >
      <h1>Mix Blend</h1>
      <Split
        :isLoading="isLoading || !image"
        :img="image"
        :isOpen="showSplit"
        @update-image="updateImage()"
      />
    </section>
    <Buttons @split="showSplit = !showSplit" @update-theme="updateTheme()" />
    <transition name="fade">
      <h2 class="doc" v-if="!showSplit">
        Learn more about mix blend mode through Mozilla's
        <a
          href="https://developer.mozilla.org/en-US/docs/Web/CSS/mix-blend-mode"
          >mix-blend-mode</a
        >
        documentation
      </h2>
    </transition>
  </main>
</template>

<script>
import { createClient } from "pexels";
import theme from "./assets/themes.json";
import Buttons from "./components/Buttons.vue";
import Split from "./components/Split.vue";

export default {
  name: "App",
  components: {
    Buttons,
    Split,
  },
  data() {
    return {
      isLoading: false,
      cardSize: null,
      showSplit: true,
      theme: theme,
      activeTheme: "web",
      image: null,
      currentImage: 0,
      images: [],
      imageClient: createClient(process.env.VUE_APP_API),
    };
  },
  methods: {
    updateTheme() {
      const { pattern, web } = this.theme;
      const newTheme = this.activeTheme === "pattern" ? pattern : web;
      this.updateColorProperties(Object.entries(newTheme));
      this.activeTheme = this.activeTheme === "web" ? "pattern" : "web";
      this.getImages();
    },
    updateImage() {
      this.isLoading = true;
      if (!this.images[this.currentImage++]) this.getImages();
      this.image = this.images[this.currentImage++];

      setTimeout(() => {
        this.isLoading = false;
      }, 1300);
    },
    getImages() {
      this.imageClient.photos
        .search({ query: this.activeTheme, per_page: 10 })
        .then(({ photos }) => {
          const [photo] = photos;
          this.images = photos;
          const image = {
            author: photo.photographer,
            authorURL: photo.photographer_url,

            src: {
              original: photo.src.original,
              landscape: photo.src.landscape,
              xl: photo.src.large2x,
              large: photo.src.large,
              small: photo.src.small,
            },
            alt: photo.alt,
          };
          this.image = image;
        });
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

      const { left, right, top, bottom } = app.getBoundingClientRect();

      const horizontalCenter = (left + right) / 2;
      const x = -(horizontalCenter - clientX);

      const verticalCenter = (top + bottom) / 2;
      const y = verticalCenter - clientY;

      app.style.setProperty("--shadowX", `${-x / 70}px`);
      app.style.setProperty("--shadowY", `${-y / 70}px`); // I use the negative values to introduce the idea of mouse weight.
      app.style.setProperty("--rotateY", `${x / 70}deg`);
      app.style.setProperty("--rotateX", `${y / 70}deg`); // I use the negative values to introduce the idea of mouse weight.
    },
  },
  created() {
    this.getImages();
  },
  mounted() {
    const isDark = window.matchMedia("(prefers-color-scheme: dark)").matches;
    if (isDark) this.updateTheme();
    this.setRect();
  },
};
</script>

<style lang="scss">
@import "./style/reset.scss";
@import "./style/typography.scss";
@import "./animation/morph.css";

:root {
  --default: rgb(185, 234, 241);
  --light: rgb(199, 248, 255);
  --dark: rgb(146, 183, 195);
  --text: rgb(44, 62, 80);
}

#app {
  font-family: "Pontano Sans", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: var(--text);
  height: 100%;
  width: 100%;
  display: grid;
  position: relative;
}

.app {
  max-width: 1200px;
  margin: auto;
  border-radius: 1rem;
  box-shadow: calc(-1 * var(--shadowX)) var(--shadowY) 30px var(--default),
    var(--shadowX) calc(-1 * var(--shadowY)) 30px var(--text);
  padding: 2.5rem;
  transition: box-shadow 0.2s cubic-bezier(0.19, 1, 0.22, 1),
    transform 0.2s ease-out;

  @media screen and (max-width: 450px) and (max-height: 500px) {
    padding: 1rem;
  }
}

.app:hover {
  transform: rotateY(var(--rotateY)) rotateX(var(--rotateX));
}

.main {
  padding: 0.5rem;

  @media screen and (min-width: 800px) {
    padding: 5rem;
  }
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

<style lang="scss" scoped>
.doc {
  padding-bottom: 5rem;
}
</style>

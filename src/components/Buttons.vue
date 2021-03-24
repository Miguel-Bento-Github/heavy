<template>
  <section>
    <h1>CSS Playground</h1>

    <div class="container">
      <button @click="$emit('split')" class="button button-a">
        Split
      </button>
      <button @click="updateTheme" class="button button-b">
        Theme
      </button>
    </div>
  </section>
</template>

<script>
  import theme from '../assets/themes.json';

  export default {
    name: 'Button',
    props: {
      msg: String,
    },
    data() {
      return {
        theme: theme,
        activeTheme: 'light',
      };
    },
    methods: {
      updateTheme() {
        const { passion, light } = this.theme;
        const newTheme = this.activeTheme === 'light' ? passion : light;
        this.updateColorProperties(Object.entries(newTheme));
        this.activeTheme = this.activeTheme === 'light' ? 'passion' : 'light';
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
    },
  };
</script>

<style scoped>
  .container {
    display: flex;
    gap: 2rem;
    justify-content: center;
  }

  .button {
    margin-top: 1rem;
    border: 0;
    border-radius: 21% 79% 21% 79% / 69% 23% 77% 31%;
    padding: 1rem;
    outline: thin;
    background: conic-gradient(from -90deg at top left, var(--light), var(--default));
    box-shadow: 8px 8px 26px var(--dark), -8px -8px 26px var(--light);
    transition: all 0.2s ease;
    color: var(--text);
    cursor: pointer;
  }

  .button:hover {
    box-shadow: 6px 6px 15px var(--dark), -6px -6px 15px var(--light);
  }

  .button:active {
    box-shadow: 3px 3px 10px var(--dark), -3px -3px 10px var(--light);
  }

  .button-a {
    animation: morph-a 10s ease-in-out infinite alternate;
  }

  .button-b {
    animation: morph-b 10s ease-in-out infinite alternate;
  }

  .button-c {
    animation: morph-c 10s ease-in-out infinite alternate;
  }
</style>

<template>
  <section>
    <h1>CSS Playground</h1>

    <div class="container">
      <button @click="$emit('split')" class="button button-a">
        Blend
      </button>
      <button @click="$emit('update-theme')" class="button button-b">
        Theme
      </button>
    </div>
  </section>
</template>

<script>
  export default {
    name: 'Button',
    props: {
      msg: String,
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
    box-shadow: 8px 8px 26px var(--dark), -8px -8px 26px var(--default);
    transition: all 0.4s ease;
    color: var(--text);
    cursor: pointer;
  }

  .button:hover {
    box-shadow: 6px 6px 15px var(--dark), -6px -6px 15px var(--default);
  }

  .button:active {
    box-shadow: 3px 3px 10px var(--dark), -3px -3px 10px var(--default);
  }

  .button-a {
    animation: morph-a 10s ease-in-out infinite alternate;
  }

  .button-b {
    animation: morph-b 10s ease-in-out infinite alternate;
  }
</style>

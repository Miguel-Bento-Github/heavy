<template>
  <section>
    <h1>CSS Playground</h1>

    <button class="button">
      {{ msg }}
    </button>
  </section>
</template>

<script>
  export default {
    name: 'HelloWorld',
    props: {
      msg: String,
    },
  };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .button {
    border: 0;
    border-radius: 21% 79% 21% 79% / 69% 23% 77% 31%;
    padding: 1rem;
    outline: thin;
    background: #add8e6;
    box-shadow: 8px 8px 26px #93b8c4, -8px -8px 26px #c7f8ff;
    transition: all 0.2s ease;
    color: #222;
    animation: morph 8s ease-in-out infinite both alternate;
  }

  .button:hover {
    border-radius: 77% 23% 33% 67% / 43% 63% 37% 57%;
    box-shadow: 6px 6px 20px #93b6c4, -6px -6px 20px #c7f8ff;
  }

  .button:active {
    border-radius: 61% 39% 21% 79% / 69% 61% 39% 31%;
    box-shadow: 3px 3px 10px #93b3c4, -3px -3px 10px #c7f8ff;
  }

  @keyframes morph {
    0% {
      border-radius: 77% 23% 33% 67% / 43% 63% 37% 57%;
    }
    100% {
      border-radius: 61% 39% 21% 79% / 69% 61% 39% 31%;
    }
  }
</style>

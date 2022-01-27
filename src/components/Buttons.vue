<template>
  <div class="container">
    <button @click="$emit('split')" class="button button-a">
      Blend
    </button>
    <button @click="$emit('update-theme')" class="button button-b">
      Theme
    </button>
  </div>
</template>

<script>
export default {
  name: "Button",
  props: {
    msg: String,
  },
};
</script>

<style lang="scss" scoped>
section {
  position: relative;
}
.container {
  position: fixed;
  z-index: 2;
  bottom: 0;
  left: 0;
  width: 100%;
  padding: 1rem;
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  pointer-events: none;

  @media screen and (min-width: 800px) {
    padding: 5rem;
  }
}

.button:hover {
  box-shadow: 6px 6px 15px var(--dark), -6px -6px 15px var(--default);
}

.button:active {
  box-shadow: 3px 3px 10px var(--dark), -3px -3px 10px var(--default);
}

.button-a {
  animation: morph-a 6s ease-in-out infinite alternate;
}

.button-b {
  animation: morph-b 6s ease-in-out infinite alternate;
}
</style>

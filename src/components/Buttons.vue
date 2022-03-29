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
  name: 'Button',
  props: {
    msg: String,
  },
}
</script>

<style lang="scss" scoped>
section {
  position: relative;
}
.container {
  position: absolute;
  z-index: 2;
  bottom: 0;
  left: 0;
  right: 0;
  width: 100%;
  max-width: 1200px;
  margin: auto;
  padding: 1rem;
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  pointer-events: none;

  @media screen and (min-width: 800px) {
    padding: 5rem;
  }
}

.button-a {
  animation: morph-a 6s ease-in-out infinite alternate;
}

.button-b {
  animation: morph-b 6s ease-in-out infinite alternate;
}
</style>

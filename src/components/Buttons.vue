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

.button {
  border: 0;
  border-radius: 21% 79% 21% 79% / 69% 23% 77% 31%;
  padding: 1rem;
  outline: thin;
  background: conic-gradient(
    from -90deg at top left,
    var(--light),
    var(--default)
  );
  box-shadow: 8px 8px 26px var(--dark), -8px -8px 26px var(--default);
  transition: all 0.2s ease;
  color: var(--text);
  cursor: pointer;
  pointer-events: all;
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

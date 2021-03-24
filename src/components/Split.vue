<template>
  <input
    ref="split"
    @input="$refs.split.style.setProperty('--value', +$refs.split.value)"
    class="split"
    type="range"
    style="--value: 50"
  />
</template>

<script>
  export default {};
</script>

<style lang="scss" scoped>
  @mixin track() {
    border: 0;
    width: 100%;
    height: 100%;
    background: url(https://images.pexels.com/photos/3800030/pexels-photo-3800030.jpeg?auto=compress&cs=tinysrgb&dpr=2&w=500)
        10% / cover,
      linear-gradient(90deg, transparent var(--p), var(--light) 0);
    background-blend-mode: exclusion;
    border-radius: 0.5rem;
  }

  @mixin thumb() {
    border: none;
    width: 0.5rem;
    height: 100%;
    background: conic-gradient(from -20deg at top left, var(--dark), var(--text));
    filter: drop-shadow(0 0 2px var(--dark));
    border-radius: 1rem;
    cursor: move;
  }

  .split {
    margin-top: 2rem;
    display: block;
    width: 100%;
    height: 30vh;
    border-radius: 1rem;
    outline: thin;
    opacity: 0.8;
    padding: 0;
  }

  [type='range'] {
    --p: calc(var(--value) * 1%);

    &,
    &::-webkit-slider-thumb,
    &::-webkit-slider-runnable-track {
      -webkit-appearance: none;
    }

    &::-webkit-slider-runnable-track {
      @include track;
    }

    &::-webkit-slider-thumb {
      @include thumb;
    }
  }
</style>

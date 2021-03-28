<template>
  <div class="container">
    <input
      ref="split"
      @input="$refs.split.style.setProperty('--value', +$refs.split.value)"
      class="split"
      type="range"
      style="--value: 50"
    />
    <img class="image" :src="url" alt="" />
  </div>
</template>

<script>
  import gsap from 'gsap';

  export default {
    name: 'Split',
    props: {
      isOpen: Boolean,
      url: String,
    },
    data() {
      return {
        timeline: gsap.timeline(),
      };
    },
    methods: {
      grow() {
        const { split } = this.$refs;
        this.timeline = gsap.from(split, {
          autoAlpha: 0,
          height: 0,
          duration: 1,
          ease: 'power1.out',
        });
      },
    },
    watch: {
      isOpen(appear) {
        if (appear) {
          this.timeline.play();
        } else {
          this.timeline.reverse();
        }
      },
    },
    mounted() {
      this.grow();
    },
  };
</script>

<style lang="scss" scoped>
  .container {
    margin-top: 2rem;
    position: relative;
    width: 100%;
    height: 30rem;
  }

  .defaults {
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    display: block;
    border-radius: 1rem;
  }

  .image {
    @extend .defaults;
    z-index: 0;
    object-fit: cover;
    object-position: center 10%;
  }

  .split {
    @extend .defaults;
    outline: thin;
    z-index: 1;
    padding: 0;
    opacity: 0.3;
  }

  @mixin track() {
    border: 0;
    width: 100%;
    height: 100%;
    background: linear-gradient(90deg, transparent var(--p), var(--default) 0);
    border-radius: 0.5rem;
    mix-blend-mode: exclusion;
  }

  @mixin thumb() {
    border: none;
    width: 2%;
    height: 100%;
    background: conic-gradient(from -20deg at top left, var(--dark), var(--text));
    filter: drop-shadow(0 0 2px var(--dark));
    border-radius: 1rem;
    cursor: move;
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

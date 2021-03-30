<template>
  <div ref="container" class="container">
    <input
      ref="split"
      @input="$refs.split.style.setProperty('--value', +$refs.split.value)"
      class="split"
      type="range"
      style="--value: 50"
    />
    <img class="image" :src="url" alt="" />
    <div ref="reload" class="reload" @click="$emit('update-image')"><Reload /></div>
  </div>
</template>

<script>
  import gsap from 'gsap';
  import Reload from '../assets/svg/Reload';

  export default {
    name: 'Split',
    components: {
      Reload,
    },
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
        const { container } = this.$refs;
        this.timeline = gsap.to(container, {
          autoAlpha: 1,
          height: '30rem',
          duration: 1,
          ease: 'power1.out',
          delay: 0.2,
        });
      },
      animateRotate() {
        const { reload } = this.$refs;
        gsap.from(reload, {});
      },
    },
    watch: {
      isOpen(appear) {
        console.log(this.isOpen);
        if (appear) {
          this.grow();
        } else {
          this.timeline.reverse();
        }
      },
    },
    mounted() {
      this.animateRotate();
    },
  };
</script>

<style lang="scss" scoped>
  .container {
    margin-top: 2rem;
    position: relative;
    width: 100%;
    height: 0;
    opacity: 0;
  }

  .defaults {
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    display: block;
    border-radius: 1rem;
    overflow: hidden;
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
    opacity: 0.9;
    background: linear-gradient(90deg, transparent var(--p), var(--text) 0);
    mix-blend-mode: difference;
  }

  @mixin track() {
    border: 0;
    width: 100%;
    height: 100%;
    background: linear-gradient(90deg, transparent var(--p), var(--default) 0);
    border-radius: 0.5rem;
  }

  @mixin thumb() {
    border: none;
    width: 1%;
    height: 100%;
    background: conic-gradient(from -20deg at top left, var(--dark), var(--text));
    filter: drop-shadow(0 0 2px var(--dark));
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

  .reload {
    position: absolute;
    height: 3rem;
    width: 3rem;
    top: 0;
    right: 0;
    z-index: 1;
    fill: #fff;
    cursor: pointer;
    filter: drop-shadow(3px 1px 4px var(--text));
    transition: transform 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275);

    &:hover {
      transform: rotate(180deg);
    }
  }
</style>

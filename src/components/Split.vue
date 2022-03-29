<template>
  <div @mouseenter="tooltip = false" ref="container" class="container">
    <input
      ref="split"
      @input="$refs.split.style.setProperty('--value', +$refs.split.value)"
      class="split"
      type="range"
      style="--value: 50"
    />
    <span v-if="tooltip" class="tooltip"
      >Drag the bar, change theme or reload</span
    >
    <img
      v-if="img"
      class="image"
      :src="img.src.original || img.src.xl || img.src.large || img.src.small"
      :alt="img.alt"
      :title="`Author ${img.author}`"
    />
    <div ref="reload" v-if="isOpen" class="reload" @click="reload()">
      <Reload />
    </div>
    <Spinner class="spinner" v-if="isLoading" />
  </div>
</template>

<script>
import gsap from 'gsap'
import Reload from '../assets/svg/Reload'
import Spinner from '../components/Spinner'

export default {
  name: 'Split',
  components: {
    Reload,
    Spinner,
  },
  props: {
    isLoading: Boolean,
    isOpen: Boolean,
    img: Object,
  },
  data() {
    return {
      timeline: gsap.timeline(),
      tooltip: true,
    }
  },
  methods: {
    grow() {
      const { container } = this.$refs
      this.timeline = gsap.to(container, {
        autoAlpha: 1,
        height: '55vh',
        marginTop: '2rem',
        ease: 'power1.out',
        duration: 1,
        delay: 0.2,
        onComplete: this.$emit('update-rect'),
      })
    },
    reload() {
      const { reload } = this.$refs
      this.$emit('update-image')

      const timeline = gsap.timeline()

      timeline.from(reload, {
        rotate: 360,
        duration: 2.5,
        ease: 'elastic.out(1, 0.3)',
      })
    },
  },
  watch: {
    isOpen(appear) {
      if (appear) {
        this.grow()
      } else {
        this.timeline.reverse()
      }
    },
  },
  mounted() {
    this.grow()
  },
}
</script>

<style lang="scss" scoped>
@import url('../animation/rotate.css');

.container {
  margin: auto;
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
}

.split {
  @extend .defaults;
  outline: thin;
  z-index: 1;
  padding: 0;
  opacity: 0.9;
  background: linear-gradient(90deg, transparent var(--p), var(--light) 0);
  mix-blend-mode: difference;
  cursor: move;
}

@mixin track() {
  border: 0;
  width: 100%;
  height: 100%;
  border-radius: 0.5rem;
}

@mixin thumb() {
  border: none;
  width: 3%;
  height: 100%;
  background: conic-gradient(from -20deg at top left, var(--dark), var(--text));
  filter: drop-shadow(0 0 2px var(--dark));

  @media screen and (min-width: 800px) {
    width: 1%;
  }
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

.spinner {
  position: absolute;
  top: 0.5rem;
  left: 0.5rem;
}

.reload {
  position: absolute;
  height: 64px;
  width: 64px;
  top: 0;
  right: 0;
  z-index: 1;
  cursor: pointer;
  fill: var(--default);
  filter: drop-shadow(3px 1px 4px var(--default));
  transition: transform 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  mix-blend-mode: difference;

  &--enlighten {
    fill: var(--text);
    filter: drop-shadow(3px 1px 4px var(--text));
  }

  &:hover {
    transform: rotate(90deg);
  }
}

.tooltip {
  position: absolute;
  z-index: 1;
  top: 24%;
  left: 51%;
}
</style>

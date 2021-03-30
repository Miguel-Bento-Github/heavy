<template>
  <div class="fingerprint-spinner">
    <div class="spinner-ring"></div>
    <div class="spinner-ring"></div>
    <div class="spinner-ring"></div>
    <div class="spinner-ring"></div>
    <div class="spinner-ring"></div>
    <div class="spinner-ring"></div>
    <div class="spinner-ring"></div>
    <div class="spinner-ring"></div>
    <div class="spinner-ring"></div>
  </div>
</template>

<style scoped>
  .fingerprint-spinner,
  .fingerprint-spinner * {
    box-sizing: border-box;
  }

  .fingerprint-spinner {
    height: 64px;
    width: 64px;
    padding: 2px;
    overflow: hidden;
    position: relative;
    filter: drop-shadow(0 0 4px var(--light));
  }

  .fingerprint-spinner .spinner-ring {
    position: absolute;
    border-radius: 50%;
    border: 2px solid transparent;
    border-top-color: var(--text);
    animation: fingerprint-spinner-animation 1.6s cubic-bezier(0.68, -0.75, 0.265, 1.75) infinite
      forwards;
    margin: auto;
    bottom: 0;
    left: 0;
    right: 0;
    top: 0;
  }

  .fingerprint-spinner .spinner-ring:nth-child(1) {
    height: calc(60px / 9 + 0 * 60px / 9);
    width: calc(60px / 9 + 0 * 60px / 9);
    animation-delay: calc(45ms * 1);
    border-top-color: var(--light);
  }

  .fingerprint-spinner .spinner-ring:nth-child(2) {
    height: calc(60px / 9 + 1 * 60px / 9);
    width: calc(60px / 9 + 1 * 60px / 9);
    animation-delay: calc(45ms * 2);
    border-top-color: var(--light);
  }

  .fingerprint-spinner .spinner-ring:nth-child(3) {
    height: calc(60px / 9 + 2 * 60px / 9);
    width: calc(60px / 9 + 2 * 60px / 9);
    animation-delay: calc(45ms * 3);
    border-top-color: var(--light);
  }

  .fingerprint-spinner .spinner-ring:nth-child(4) {
    height: calc(60px / 9 + 3 * 60px / 9);
    width: calc(60px / 9 + 3 * 60px / 9);
    animation-delay: calc(45ms * 4);
    border-top-color: var(--default);
  }

  .fingerprint-spinner .spinner-ring:nth-child(5) {
    height: calc(60px / 9 + 4 * 60px / 9);
    width: calc(60px / 9 + 4 * 60px / 9);
    animation-delay: calc(45ms * 5);
    border-top-color: var(--default);
  }

  .fingerprint-spinner .spinner-ring:nth-child(6) {
    height: calc(60px / 9 + 5 * 60px / 9);
    width: calc(60px / 9 + 5 * 60px / 9);
    animation-delay: calc(45ms * 6);
    border-top-color: var(--default);
  }

  .fingerprint-spinner .spinner-ring:nth-child(7) {
    height: calc(60px / 9 + 6 * 60px / 9);
    width: calc(60px / 9 + 6 * 60px / 9);
    animation-delay: calc(45ms * 7);
    border-top-color: var(--dark);
  }

  .fingerprint-spinner .spinner-ring:nth-child(8) {
    height: calc(60px / 9 + 7 * 60px / 9);
    width: calc(60px / 9 + 7 * 60px / 9);
    animation-delay: calc(45ms * 8);
    border-top-color: var(--dark);
  }

  .fingerprint-spinner .spinner-ring:nth-child(9) {
    height: calc(60px / 9 + 8 * 60px / 9);
    width: calc(60px / 9 + 8 * 60px / 9);
    animation-delay: calc(45ms * 9);
    border-top-color: var(--dark);
  }
</style>

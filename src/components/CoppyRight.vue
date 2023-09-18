<template>
  <p class="copyright">
    This game owned by Hanvu2k in Vue3 course - <a href="#">view here</a>
  </p>
</template>

<style lang="scss" scoped>
.copyright {
  position: fixed;
  left: 50%;
  transform: translateX(-50%);
  bottom: 1.5rem;
  color: var(--light);
  z-index: 3;
  font-style: 1.5rem;

  a {
    color: #f4dc26;
  }
}
</style>

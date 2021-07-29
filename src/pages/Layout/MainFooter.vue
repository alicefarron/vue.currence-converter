<template>
  <footer class="main-footer">
    <h2 class="main-footer__title">Спасибо за внимание и удачного дня</h2>
  </footer>

</template>

<script>
  export default {
    data() {
      return {
      };
    },
  };
</script>

<style lang="scss">
  .main-footer {
    display: flex;
    width: 100%;
    background-color: rgb(51, 48, 48);
    min-height: 40px;
    padding: 0 30px;

    &__title {
      font-size: 0.9rem;
      color: #fff;
    }
  }
</style>
<template>
  <footer class="footer" :class="$style.footer_main">
    <div class="content has-text-centered">
      <p>
        <strong>Prueba destacame</strong> by
        <a href="https://jgthms.com">Natacha Avilés</a>.
        <span class="ec ec-sparkling-heart"></span>
      </p>
    </div>
  </footer>
</template>

<style lang="scss" module>
@import '@design';
.footer_main {
  min-height: 50px;
  background-color: #ffffff;
  color: #223344;
  padding: 20px 0;
  margin-top: 10em;
}
</style>

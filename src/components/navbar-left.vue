<script>
export default {}
</script>

<template>
  <div class="column is-3">
    <div :class="$style.navbarleft">
      <router-link to="busAdmin">Horario</router-link>
      <router-link to="choferAdmin">Chofer</router-link>
      <router-link to="trayectoAdmin">Trayecto</router-link>
    </div>
  </div>
</template>

<style lang="scss" module>
@import '@design';
.navbarleft {
  height: 100vh;
  background-color: #ffffff;
}
.navbarleft a {
  display: block;
  padding: 15px;
  border-bottom: 1px solid #333;
}
</style>

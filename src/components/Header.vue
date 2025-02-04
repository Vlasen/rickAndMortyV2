<script setup>
</script>

<template>
  <header class="headerBox">
    <img alt="Rick and Morty Logo" 
          class="logo" 
          src="@/assets/logo.png"/>
  </header>
</template>

<style>
.headerBox {
  display: block;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 10vh;
  min-width: 500px;
  z-index: 2;
}
.logo {
  height: 9vh;
  min-height: max(60px, 5vh);
  max-height: 80px;
  margin-left: max(70px, 20vw - 150px);
}
</style>
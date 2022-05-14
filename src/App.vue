<template>
  <div class="nav">
    <h2>Todos</h2>
  </div>
  <router-view/>
</template>

<style>
#app {
  padding: 0;
  margin: 0;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>

<style scoped>
.nav{
  text-align: center;
  padding: 0;
  display: inline;
  color: grey;
  text-decoration: underline;
  text-decoration-color: rgb(39, 170, 126);
  text-underline-offset: 4px;
}
.nav h2{
  margin-top: 50px;
}
</style>
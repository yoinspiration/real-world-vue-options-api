<script>
export default {
  inject: ['GStore']
}
</script>

<template>
  <div id="flashMessage" v-if="GStore.flashMessage">
    {{ GStore.flashMessage }}
  </div>
  <nav>
    <router-link :to="{ name: 'EventList' }">Events</router-link> |
    <router-link :to="{ name: 'About' }">About</router-link>
  </nav>
  <router-view />
</template>

<style>
@keyframes yellowFade {
  from {
    background: yellow;
  }
  to {
    background: transparent;
  }
}

#flashMessage {
  animation-name: yellowFade;
  animation-duration: 3s;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a,
#nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active,
#nav a.router-link-exact-active {
  color: #42b983;
}

body {
  padding-bottom: 50px;
}
</style>

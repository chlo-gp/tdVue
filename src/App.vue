<template>
  <div id="nav">
    <router-link to="/">
      Home
    </router-link> |
    <router-link to="/users">
      Users
    </router-link> |
    <router-link to="/users/:id">
      About
    </router-link>
  </div>
  <router-view />
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  position: absolute;
  padding: 30px 0;
  top: 0;
  z-index: 2;
  width: 100%;
}

#nav a {
  font-weight: bold;
  color: black;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>


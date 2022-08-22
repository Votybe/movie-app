<template>
  <nav>
    <router-link to="/">Home</router-link>
    <router-link to="/about">Favoris</router-link>
  </nav>
  <router-view />
</template>

<style lang="scss">
* {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}

body {
  font-family: "Noto Serif", serif;
}

nav {
  padding: 30px;
  background-color: #13171a;
  display: flex;
  justify-content: center;

  a {
    color: #fff;
    font-weight: bold;
    margin: 0.3rem;
    text-decoration: none;

    &.router-link-exact-active {
      text-decoration: underline;
    }
  }
}
</style>

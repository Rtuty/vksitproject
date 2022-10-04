<template>
  <div id="app">
    <nav>
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </nav>
    <router-view/>
  </div>
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ffffff;
}

nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #ffffff;

    &.router-link-exact-active {
      color: #2ae09f;
    }
  }
}
textarea {
  max-width: 25%;
}

textarea.form-control {
  max-width: 20%;
}

.main {
  display: grid;
  place-items: center;
}

body {
  background-image: url('~@/assets/cryptoground.png') !important;
}

p {
  margin-top: 1% !important;
  /*margin-bottom: 5% !important; */
}

label {
  /* margin-top: 5%; */
  color: white;
}

.main {
  margin-top: 10% !important;
}
</style>

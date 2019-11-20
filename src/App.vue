<template>
  <div id="app">
    <div v-if="username" class="pseudal shadow-lg" >User: {{username}}</div>
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/admin">Admin</router-link> |
      <router-link to="/questionnaire">Questionnaire</router-link>
    </div>
    <router-view/>
  </div>
</template>

<style lang="scss">
@import "./assets/css/custom.scss";
@import "../node_modules/bootstrap/scss/bootstrap.scss";
@import "./assets/css/login.css";
@import "./assets/css/style.css";

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #ff9633;
    }
  }
}
</style>
<script>

export default {
  data () {
    return {
      username: null
    }
  },
  created: function () {
    this.loadPseudal()
  },
  watch: {
    // au changement de route
    $route (to, from) {
      this.loadPseudal()
    }
  },
  methods: {
    // on vérifie si le pseudal de l'user est save dans localStorage
    loadPseudal: function () {
      if (localStorage.username && localStorage.username !== undefined) {
        this.username = localStorage.username
      }
    }
  }
}
</script>

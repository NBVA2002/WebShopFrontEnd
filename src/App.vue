<template>
  <div>
    <base-nav class="navbar" :islogin="isLogin"/>
    <router-view class="view" :islogin="isLogin"/>
    <footer-place class="footer" />
  </div>
</template>

<script>
import NavBar from "./components/NavBar.vue";
// import axios from "axios";
import FooterPlace from "./components/FooterPlace.vue";
export default {
  name: "App",
  components: {
    BaseNav: NavBar,
    FooterPlace,
  },

  data() {
    return {
      user: {},
      isLogin: false,
      isAdmin: false,
    };
  },

  created() {
    this.isLogin = this.checkLogin();
    console.log("is Login: " + this.isLogin);
  },

  methods: {
    checkLogin() {
      if (localStorage.getItem("token")) {
        return true;
      }
      return false;
    },
  },
};
</script>

<style>
#app {
  font-family: var(--font);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.navbar {
  position: fixed;
  top: 0;
  z-index: 1;
}

.view {
  margin-top: 70px;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>

<template>
    <div id="app">
        <div id="nav">
            <router-link to="/Login">Login</router-link>
            |
            <router-link to="/">Home</router-link>
 
            <span align-end style="cursor: pointer;">
            <template v-if="!user.isAuthenticated">
            <span icon="person"></span>
            </template>
            <template v-else>
            <span style="color: darkcyan;">{{ user.name }}</span>
            <span  icon="person"></span>
            <span @click="logout">  退出</span>
            </template>
            </span>
        </div>
        <router-view/>
    </div>
</template>

<script>
import applicationUserManager from "./Auth/applicationusermanager";
import userAuth from "./Auth/UserAuth";

export default {
  name: "app",
  mixins: [userAuth],
  data: function() {
    return {};
  },
  watch: {
    $route: async function(to, from) {
    }
  },
  methods: {
    async login() {
      try {
        await applicationUserManager.login();
      } catch (error) {
        console.log(error);
        this.$root.$emit("show-snackbar", { message: error });
      }
    },
    async logout() {
      try {
        await applicationUserManager.logout();
        this.$store.commit("saveToken", "");
      } catch (error) {
        console.log(error);
        this.$root.$emit("show-snackbar", { message: error });
      }
    }
  }
};
</script>

<style lang="css">
    @import "./style/stylehome.css";
</style>


<style lang="stylus">
    #app
        font-family 'Avenir', Helvetica, Arial, sans-serif
        -webkit-font-smoothing antialiased
        -moz-osx-font-smoothing grayscale
        color #2c3e50

    #nav
        padding 30px
        text-align center

    a
        font-weight bold
        color #2c3e50
        &.router-link-exact-active
            color #42b983
</style>

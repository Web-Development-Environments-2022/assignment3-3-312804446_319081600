<template>
  <div id="app">
    <div id="nav">
      <router-link :to="{ name: 'main' }">Vue Recipes</router-link>|
      <router-link :to="{ name: 'search' }">Search</router-link>|
      <!-- this is the false close to the username -->
      {{ !$root.store.username }}
      <span v-if="!$root.store.username">
        Guest:
        <router-link :to="{ name: 'register' }">Register</router-link>|
        <router-link :to="{ name: 'login' }">Login</router-link>|
      </span>
      <span v-else>
        <select name="format" id="format" v-on:change="changeRoute($event)">
          <option ></option>
          <option value="favorites" >favorites</option>
          <option value="MyRecipes">MyRecipes</option>
          <option value="familyRecipes">familyRecipes</option>
        </select>
        {{ $root.store.username }}: <button @click="Logout">Logout</button>|
      </span>
 
    </div>
    <router-view />
  </div>
</template>


<script>
export default {
  // data() {

  // },
  methods: {
    changeRoute(e) {
      this.$router.push("/users/" + e.target.value);
    },
    Logout() {
      this.$root.store.logout();
      this.$root.toast("Logout", "User logged out successfully", "success");
      this.$router.push("/").catch(() => {
        this.$forceUpdate();
      });
    },

  },
};
</script>

<style>
#nav {
  padding-top: 10px;
  padding-bottom: 10px;
  margin-left: 0 !important;
  height: 62px;
  width: 100%;
  background-color: whitesmoke;
  font-family: 'Palanquin Dark';
  justify-content: center;
  font-size: 22px;
  -webkit-box-shadow: -2px 2px 9px 7px rgba(0,0,0,0.53);
  -moz-box-shadow: -2px 2px 9px 7px rgba(0,0,0,0.53);
  box-shadow: -2px 2px 9px 7px rgba(0,0,0,0.53);
  z-index: 10;
}
#nav a {
  color: #333333;
  padding: 10px;
  margin: 0;
}
#nav a:hover {
  opacity: 70%;
  text-decoration: none;
}
#nav a.router-link-exact-active {
  opacity: 70%;
}
#my-logo{
  padding: 10px;
  padding-bottom: 10px;
  margin-right: 20px;
  margin-left: -15px !important;
  padding-left: 35px;
  padding-right: 25px;
  font-size: 120%;
  background-color: #333333;
  color: whitesmoke !important;
}
#my-logo:hover{
  text-decoration: none;
  opacity: 90%; 
}
#my-logo.router-link-exact-active{
  text-decoration: none;
  opacity: 100% !important;
}
.sticky {
  position: fixed;
  top: 0;
  width: 100%;
}
#user-section{
  text-align: right;
  justify-content: center;
  color: #333333;
  padding-bottom: 3px;
}
#main-section a{
  margin-left: 20px;
}
.user-section{
  margin-left: 30px;
  font-size: 22px;
}
#user-section{
  display: flex;
  flex-direction: column;
  justify-content: center;
}
#logout-btn,
#logout-btn:focus
{
  outline: none;
  border: none;
}
#logout-btn:hover {
  opacity: 70%;
}
.hello {
  padding-right: 25px;
}
#personal {
  font-family: 'Palanquin Dark';
}
#personal .dropdown-menu {
  background-color: whitesmoke;
  padding: 3px;
  border: none;
}
#personal a {
  margin-left: 0;
  font-size: 20px !important;
}
#personal button::after {
  display: none;
}
#personal button:focus,
#personal button {
  background-color: transparent;
  color: #333333;
  margin-left: 20px;
  outline: none;
  border: none;
  box-shadow: none;
  font-size: 22px;
  padding: 10px;
  padding-top: 4px;
}
</style>

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
        <CreateRecipePage/>
        {{ $root.store.username }}: <button @click="Logout">Logout</button>|
      </span>
    </div>
    <router-view />
  </div>
</template>


<script>
import CreateRecipePage from '../pages/CreateRecipePage.vue';
export default {
  name: "NavBar",
  components: {
    CreateRecipePage
  },
  data() {
    return {
      // form: {
      //   recipename: '',
      //   servings: '',
      //   readyInMinutes:'',
      //   image:'',
      //   checked_gluten: false,
      //   checked_vegetarian: false,
      //   checked_vegan: false,
      //   insrtaction:'',

      //   },
      // form2: {
      //   ingredient:'',
      //   amount:''
      // },
      //   ingredients:[],
      //   display_ingredients:""
      }
    },
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


</style>

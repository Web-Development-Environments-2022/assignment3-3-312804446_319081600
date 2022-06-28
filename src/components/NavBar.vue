


<template>
  <!-- <b-navbar type="dark" variant="info" class="nav-bar"> -->
  <b-navbar  type="dark" variant="dark" class="nav">    
    <router-link tag="b-navbar-brand" style="color:#FAEBD7" :to="{ name: 'main' }"
     
      >    <img src="https://res.cloudinary.com/dfgjujaok/image/upload/c_scale,h_40/v1594843880/logo3_aqd4ac.jpg">
      <!-- <b>Vue Recipes</b> -->
    </router-link
    >
    <!-- Home -->
    <b-navbar-nav>
      <router-link tag="b-nav-item" :to="{ name: 'main' }"><b>Home</b></router-link>
    </b-navbar-nav>

    <!-- Search -->
    <b-navbar-nav>
      <router-link tag="b-nav-item" :to="{ name: 'search' }"
        ><b>Search</b>
      </router-link
      >
    </b-navbar-nav>

    <!-- About -->
    <b-navbar-nav>
      <router-link tag="b-nav-item" to="/About"
        ><b>About</b></router-link
      >
    </b-navbar-nav>

    <!-- Registred user  -->
    <b-navbar-nav v-if="$root.store.username">  
      <!-- dropdown Myreciprs : Favorites,Private,Family -->
      <b-nav-item-dropdown text="more" class="bold-option">
        <!-- <div v-if="this.$root.store.username"> -->
        <router-link tag="b-dropdown-item" to="/users/favorites"
          ><b>Favorites</b></router-link
        >     
        <router-link tag="b-dropdown-item" to="/users/MyRecipes"
          ><b>My Recupes</b></router-link
        >
        <router-link tag="b-dropdown-item" to="/users/familyRecipes"
          ><b>My Family's Recipes</b></router-link
        >
      </b-nav-item-dropdown>
      <CreateRecipePage/>
    </b-navbar-nav>

    <!-- dropdown Hello guest: Login & Register -->
    <b-navbar-nav class="ml-auto">
      <b-nav-item-dropdown class="bold-option" 
        v-if="!$root.store.username"
        right text="Hello guest" 
      >
        <router-link tag="b-dropdown-item" :to="{ name: 'register' }"
          ><b>Register</b></router-link
        >
        <router-link tag="b-dropdown-item" :to="{ name: 'login' }"
          ><b>Login</b></router-link
        >
      </b-nav-item-dropdown>

      <!-- Logout -->
      <span v-else>
         <b-nav-item>
          Hello {{ $root.store.username }}:<button @click="Logout" class="button"><b>Logout</b></button>
         </b-nav-item>
      </span>
    </b-navbar-nav>
  </b-navbar>
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
.nav-bar{
font-family:"Comic Sans MS", cursive, sans-serif;
}
.bold-option{
  font-weight: bolder;
}
.button{
  color:#FAEBD7;
}
.button:hover{
  color:#9aaeb6;
}
</style>

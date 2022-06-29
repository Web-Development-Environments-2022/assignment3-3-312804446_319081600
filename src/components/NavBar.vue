

<template>
  <!-- <b-navbar type="dark" variant="info" class="nav-bar"> -->
    <VueScrollFixedNavbar>
  <b-navbar toggleable="lg"  class="nav-bar">    
    <router-link tag="b-navbar-brand" :to="{ name: 'main' }"
     
      >    <img src="https://res.cloudinary.com/dqwbwwfui/image/upload/v1656438855/samples/logo1_kswuhs.png">
      <!-- <b>Vue Recipes</b> -->
    </router-link>
    <!-- Home -->
    <b-navbar-nav>
      <router-link tag="b-nav-item" :to="{ name: 'main' }"><b>Home</b></router-link>
    </b-navbar-nav>

    <!-- Search -->
    <b-navbar-nav>
      <router-link tag="b-nav-item" :to="{ name: 'search' }"
        ><b>Search</b>
      </router-link >
    </b-navbar-nav>

    <!-- About -->
    <b-navbar-nav>
      <router-link tag="b-nav-item" to="/About"
        ><b>About</b></router-link>
    </b-navbar-nav>

    <!-- Registred user  -->
    <b-navbar-nav v-if="$root.store.username">  
          <b-navbar-nav>
        <!-- <CreateRecipePage/> -->
        <router-link v-b-modal.modal-1 id="modal-1" tag="b-nav-item" to="/users/CreateRecipe"
          ><b>New Recipes</b></router-link>
      </b-navbar-nav>
      
      <!-- dropdown Myreciprs : Favorites,Private,Family -->
      <b-nav-item-dropdown text="more" class="bold-option">
        <!-- <div v-if="this.$root.store.username"> -->
        <router-link tag="b-dropdown-item" to="/users/favorites"
          ><b>Favorites</b></router-link>     
        <router-link tag="b-dropdown-item" to="/users/MyRecipes"
          ><b>My Recupes</b></router-link>
        <router-link tag="b-dropdown-item" to="/users/familyRecipes"
          ><b>My Family's Recipes</b></router-link>
      </b-nav-item-dropdown>

    </b-navbar-nav>

    <!-- dropdown Hello guest: Login & Register -->
    <b-navbar-nav class="ml-auto">
      <b-nav-item-dropdown class="bold-option" 
        v-if="!$root.store.username"
        right text="Hello guest" >
        <router-link tag="b-dropdown-item" :to="{ name: 'register' }"
          ><b>Register</b></router-link>
        <router-link tag="b-dropdown-item" :to="{ name: 'login' }"
          ><b>Login</b></router-link>
      </b-nav-item-dropdown>

      <!-- Logout -->
      <span v-else>
         <b-nav-item>
         <b-row> 
          <b-col>
            <b-navbar-item><h3>Hello {{ $root.store.username }}   </h3></b-navbar-item>  
         </b-col> 
         <router-link v-b-modal.modal-1 id="modal-1" tag="b-nav-item" to="/" @click.native="Logout()" ><b>Logout</b></router-link></b-row>
          <!-- <button @click="Logout" id="button"><b>Logout</b></button> -->
         </b-nav-item>
      </span>
    </b-navbar-nav>
  </b-navbar>
  </VueScrollFixedNavbar>
</template>


<script>
import CreateRecipePage from '../pages/CreateRecipePage.vue';
export default {
  name: "NavBar",
  components: {
    // CreateRecipePage
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
  /* color:#FAEBD7 */
  background-color: #F19CBB!important;
  height:50px ;


}
.nav-bar.navbar-dark.bg-dark{
    background-color: #AABB55!important;
 }

.bold-option{
  font-weight: bolder;
}
</style>

<template>
    <center>
      <br/>
      <br/>
    <div class="recipe-footer">
      <div :title="recipe.title" class="recipe-title" style="text-decoration-line: underline;">
        {{ recipe.title }}
      </div>
      <br/>
      <div class="recipe-body">
          <!-- <img v-if="image_load" :src="recipe.image" class="recipe-image" /> -->
          <img :src="recipe.image" class="recipe-image" />
      </div>
      <br/>
      <ul class="recipe-overview">
        <b-list-group >
          <dt>{{ recipe.readyInMinutes }} minutes to prepare</dt>
          <dt>{{ recipe.aggregateLikes }} likes this recipe</dt>
          <dt v-if="recipe.vegan">Vegan</dt>
          <dt v-if="recipe.vegetarian"> Vegeterian</dt>
          <dt v-if="recipe.glutenFree"> Gluten Free</dt>
          <!-- <li>{{ recipe.instruction }} instruction</li> -->
          <dt v-if="recipe.is_favorite">Favorite Recipe</dt>
          <dt v-if="recipe.is_watched">Viewed Recipe</dt>
          <dt>{{ recipe.servings }} servings</dt>
          <center>
             <b-button v-if="API_route" id="favoritesButton" size="sm" @click="AddToFavorites" class="w-15 h-20"> add to favorites</b-button>
          </center>
        </b-list-group >
        <br/>
        <div id="instructions"> <h3 id="InstructionsTitle">Instructions</h3> <br/>
           {{ instructions }}</div>
      </ul>
  </div>
  </center>
</template>

<script>
// import RecipePreview from "./RecipePreview.vue";
export default {
  name: "Recipeview",
  data() {
    return {
      recipe: {},
      instructions:"",
      recipe_id:"",
      API_route: false,
    };
  },
  mounted() {
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {

      try {
        // console.log(this.$route.path)
        let split_route = (((this.$route.path).split("/")[2]).split("%2F"))[2]
        let split_recipe_id = ((this.$route.path).split("/"))[4]
         if(split_route == "lastWatched" || split_route =="random" || split_route =="favorites" || split_route =="search"){
          this.API_route = true;
          const response = await this.axios.get(
          this.$root.store.server_domain + "/recipes/FullRecipeDetailsAPI/" + split_recipe_id)
          let array_steps = response.data.instructions
          // console.log(response.data.instructions);
          for (const analyze of array_steps){
            for(const ste of analyze.steps){
              this.instructions = this.instructions + ste.step
            }
          }
          console.log(response);
          this.recipe= response.data;
          this.recipe_id= response.data.id;        
        }
        else{
          // console.log("else:")
          const response = await this.axios.get(
          this.$root.store.server_domain + "/users/FullRecipeDetailsDB/" + split_recipe_id)
          console.log(response);

          this.recipe= response.data[0];
          this.instructions = response.data[0].instructions;
          this.recipe_id= response.data[0].id;
        }
        //add to last watch table
        if(this.$root.store.username){ //is user
          const response = await this.axios.post( 
          this.$root.store.server_domain + "/users/lastWatched/",
          {
            recipe_id: this.recipe_id,
          }
          );
          console.log(response)
        }
      } catch (error) {
        console.log(error);
      }
    },
    async AddToFavorites(){
       const response = await this.axios.post(
          // "https://test-for-3-2.herokuapp.com/user/Register",
          this.$root.store.server_domain + "/users/favorites",
          {
            recipe_id: this.recipe_id
          }
        );
    }
  }
};
</script>

<style scoped>
.favoritesButton{
  background-color:  #F19CBB;
  color: #0ad8d8;
}
 .recipe-title{
    color: #2f4f4f;
  font-weight: bolder;
 font-size: 32px;

 }
.recipe-overview{
  font-size: 20px;
}
#InstructionsTitle{
  font-weight: bolder;
  color: #2f4f4f;
  text-decoration-line: underline;
}
 #instructions{
  max-width: 800px; 
  width:800px; 
 }

 .recipe-footer{
background: #F19CBB;
background: -moz-linear-gradient(-45deg, #F19CBB 0%, #F9CB95 53%, #F19CBB 100%);
background: -webkit-linear-gradient(-45deg, #F19CBB 0%, #F9CB95 53%, #F19CBB 100%);
background: linear-gradient(135deg, #F19CBB 0%, #F9CB95 53%, #F19CBB 100%);
   max-width: 1000px; 
  width:1000px; 
  padding: 1em;
 }
</style>

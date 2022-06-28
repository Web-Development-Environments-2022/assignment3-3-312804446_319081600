<template>
    <div class="recipe-footer">
      <div :title="recipe.title" class="recipe-title">
        {{ recipe.title }}
      </div>
      <div class="recipe-body">
          <!-- <img v-if="image_load" :src="recipe.image" class="recipe-image" /> -->
          <img :src="recipe.image" class="recipe-image" />
      </div>
      <ul class="recipe-overview">
        <li>{{ recipe.readyInMinutes }} minutes</li>
        <li>{{ recipe.aggregateLikes }} likes</li>
        <li>{{ recipe.vegan }} vagen</li>
        <li>{{ recipe.vegetarian }} vageterian</li>
        <li>{{ recipe.glutenFree }} Gluten Free</li>
        <li> instructions ----> {{ instructions }}</li>
        <!-- <li>{{ recipe.instruction }} instruction</li> -->
        <li>{{ recipe.is_favorite }} is_favorite</li>
        <li>{{ recipe.is_watched }} is_watched</li>
        <li>{{ recipe.servings }} servings</li>

        <!-- <li>{{ recipe.extendedIngredients }} extendedIngredients</li> -->

      </ul>
  </div>
</template>

<script>
// import RecipePreview from "./RecipePreview.vue";
export default {
  name: "Recipeview",
  // components: {
  //   RecipePreview
  // },
  // props: {
  //   title: {
  //     type: String,
  //     required: true
  //   },
  //   route_name:{
  //     type: String,
  //     required: true    
  //   }
  // },
  data() {
    return {
      recipe: {},
      instructions:"",
      recipe_id:""

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
    }
  }
};
</script>




<style scoped>


</style>
>
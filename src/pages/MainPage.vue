<template>
  <div class="container">
    <br/>
    <br/>
    <center>
      <h1 class="title">Welcome to Tasty Recipes</h1>
    </center>
    <br/>
    <br/>
    <b-row>
      <b-col >
    <RecipePreviewList route_name="/recipes/random" title="Random Recipes" class="RandomRecipes center" />
      </b-col>
      <b-col >
         <router-link v-if="!$root.store.username" to="/login" tag="button">You need to Login to vue this</router-link>
        <!-- this is the false above ths title of last viewed -->
        <RecipePreviewList route_name="/users/lastWatched" title="Last Viewed Recipes" class="LastViewedRecipes"
          :class="{
            // RandomRecipes: true,
            blur: !$root.store.username,
            center: true
        }"/>
      </b-col>
    </b-row>
  </div>
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList";
// import LastWatchedRecipePreviewList from "../components/LastWatchedRecipePreviewList";

export default {
  components: {
    RecipePreviewList,
   
  }
};
</script>

<style lang="scss" scoped>
.RandomRecipes {
  margin: 0px -350px 0px;
}
.LastViewedRecipes{
  margin: 0px -50px 0px;

}
.blur {
  // -webkit-filter: blur(5px); /* Safari 6.0 - 9.0 */
  // filter: blur(2px);
  opacity: 0;
}
::v-deep .blur .recipe-preview {
  pointer-events: none;
  cursor: default;
}

</style>

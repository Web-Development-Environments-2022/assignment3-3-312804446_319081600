<template>
  <div class="container">
    <br/>
    <br/>
    <center>
      <h1 class="title">Welcome to Tasty Recipes</h1>
    </center>
    <br/>
    <br/>
    <center>
    <table class="center" style="align=center; ">
            <!-- <tbody> -->
              <center>
                 <tr>
                  <td >
                    <RecipePreviewList route_name="/recipes/random" title="Random Recipes" class="RandomRecipes center" />
                  </td>
                  <td>
                    <RecipePreviewList route_name="/users/lastWatched" title="Last Viewed Recipes" class="LastViewedRecipes center"
                    :class="{
                      // RandomRecipes: true,
                      blur: !$root.store.username,
                      center: true}"/>
                  </td >
              </tr>
                </center>
             
            <!-- </tbody> -->
          </table>

    </center>



   

    <!-- <b-row>
      <b-col  class="RandomRecipesCol">
        <RecipePreviewList route_name="/recipes/random" title="Random Recipes" class="RandomRecipes center" />
      </b-col>
      <b-col  >
        <router-link v-if="!$root.store.username" to="/login" tag="button">You need to Login to vue this</router-link> -->
        <!-- this is the false above ths title of last viewed -->
        <!-- <RecipePreviewList route_name="/users/lastWatched" title="Last Viewed Recipes" class="LastViewedRecipes center"
          :class="{
            // RandomRecipes: true,
            blur: !$root.store.username,
            center: true}"/>
      </b-col>
    </b-row> -->
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
// .RandomRecipesCol{}
// .RandomRecipes {
//   margin: 0px -350px 0px;
// }
// .LastViewedRecipes{
//   margin: 0px -50px 0px;

// }
.center {
  margin-left: auto;
  margin-right: auto;
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
.title{
  color:#F2975B;
  font-weight: bolder;

}
.big-title{
  font-family:Georgia, 'Times New Roman', Times, serif;
  font-size: 48px;
  color:#F25360;
}

</style>

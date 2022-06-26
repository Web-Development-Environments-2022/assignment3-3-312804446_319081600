<template>
    <div class="container">
      <RecipePreviewList route_name="/users/favorites" title="Favorite Recipes" class="FavoriteRecipes center" />
  </div>
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList";

export default {
  name: "favorites",
  components: {
    RecipePreviewList
  }
};
</script>

<style>

</style>
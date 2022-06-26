<template>
    <div class="container">
      <RecipePreviewList route_name="/users/CreateRecipe" title="My Recipes" class="MyRecipes center" />
  </div>
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList";

export default {
  name: "MyRecipes",
  components: {
    RecipePreviewList
  }
};
</script>

<style>

</style>
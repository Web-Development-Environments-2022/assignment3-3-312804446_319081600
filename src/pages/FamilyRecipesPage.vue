<template>
    <div class="container">
      <RecipePreviewList route_name="/users/familyRecipes" title="Family Recipes" class="FamilyRecipes center" />
  </div>
</template>


<script>
import RecipePreviewList from "../components/RecipePreviewList";

export default {
  components: {
    RecipePreviewList
  }
};
</script>

<style>

</style>
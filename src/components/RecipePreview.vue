<template>
 <router-link
    :to="{ name: 'recipe', params: { recipeId: recipe.id, route_name: route_name} }" class="recipe-preview">
  
<div>
  <b-card 
    pill
    class="recipePreviewDetails" 
    :title= "recipe.title"
    :img-src="recipe.image"
    img-alt="Image"
    img-top
    tag="article"
    style="max-width: 33rem;"
  >
    <b-card-text class="recipePreviewDetailsText">
      <b-list-group >
        <dt>{{ recipe.readyInMinutes }} minutes to prepare</dt>
        <dt>{{ recipe.servings }} servings</dt>
        <dt>{{ recipe.aggregateLikes }} <img src="../assets/like.png" style="width: 22px"/> this recipe</dt>
        <dt v-if="recipe.vegan"><img src="../assets/vegan.png" style="width: 30px"/>   Vegan</dt>
        <dt v-if="recipe.vegetarian"><img src="../assets/vegetarian.png" style="width: 30px"/> Vegeterian</dt>
        <dt v-if="recipe.glutenFree"><img src="../assets/gluten-free.png" style="width: 25px"/> Gluten Free</dt>
      </b-list-group>
    </b-card-text>
  </b-card>
</div>
</router-link>
</template>

<script>
export default {

  data() {
    return {
    };
  },
  props: {
    recipe: {
      type: Object,
      required: true
    },
    title: {
      type: String,
      required: true
    },
    route_name:{
      type: String,
      required: true
    },
  }
};
</script>

<style scoped>
.recipePreviewDetails{
  
  background-color: #C4FAF8;
  border-radius:30px;
  /* background-color: rgb(251, 248, 157); */
}
.recipePreviewDetailsText{
  color: black;
  
}
</style>

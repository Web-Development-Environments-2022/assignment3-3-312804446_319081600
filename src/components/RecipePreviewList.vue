<template>
  <b-container>
    <br/>
    <br/>
    <center>
      <h1 class="title text-center" style="  color: #2f4f4f; font-weight: bolder;">
        {{ title }}:
      <slot></slot>
    </h1>
    </center>
   <center>
    <br/>
    <br/>
    <br/>
    <br/>
    <br/>
    <h1  v-if="no_recipe">There Are No {{ title }}</h1>
    </center>
    <center>
    <b-row v-for="r in recipes" :key="r.id">
      <RecipePreview class="recipePreview" :recipe="r" :title="title" :route_name="route_name"/>
    </b-row>
     </center>
  </b-container>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
export default {
  name: "RecipePreviewList",
  components: {
    RecipePreview
  },
  props: {
    title: {
      type: String,
      required: true
    },
    route_name:{
      type: String,
      required: true    
    }
  },
  data() {
    return {
      recipes: [],
      no_recipe: false
    };
  },
  mounted() {
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {
      try {
        const response = await this.axios.get(
          this.$root.store.server_domain + this.route_name,
          // {withCredentials: true}
          // "https://test-for-3-2.herokuapp.com/recipes/random"
        );
        // console.log(response);
        if(response.data.length === 0) {
          this.no_recipe = true;
        }
        // console.log(response);
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
        // console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}
</style>

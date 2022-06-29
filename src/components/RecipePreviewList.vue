<template>
  <b-container>
    <br/>
    <br/>
    <center>
      <h3 class="big-title text-center" >
        {{ title }}
      <slot></slot>
    </h3>
    </center>
   <center>
 
    <h1  v-if="no_recipe">
    <br/>
    <br/>
    <br/>
    <br/>
    <br/>
    There Are No {{ title }}</h1>
    </center>
    <center>
    <b-row v-for="r in recipes" :key="r.id">
      <RecipePreview class="recipePreview" :recipe="r" :title="title" :route_name="route_name" style="margin-left:150px;"/>
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


.big-title{
  font-family:Georgia, 'Times New Roman', Times, serif;
  font-size: 48px;
  color:#F25360;
  font-weight: bolder;
}
</style>

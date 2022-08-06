<template>
  <b-container>
    <center>
    <h3 class="big-title text-center" >
        {{ title }}
      
      <slot></slot>
      <br/>
      <br/>
      <br/>
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
    <b-col v-for="r in recipes" :key="r.id">
      <RecipePreview class="recipePreview" :recipe="r" :title="title" :route_name="route_name" style="margin-left:205px;"/>
      
    </b-col>
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
  min-height: 80px;
}

.big-title{
  font-family:Georgia, 'Times New Roman', Times, serif;
    position: absolute;
    // top: 5%; 
    right: 50%;
    transform: translate(50%,-50%);
    text-transform: uppercase;
    // font-family: verdana;
    font-size: 2em;
    font-weight: 100;
    color: #F25360;
    text-shadow: 1px 1px 1px #919191,
        1px 2px 1px #F25360,
        1px 3px 1px #F25360,
        1px 4px 1px #919191,
        1px 5px 1px #919191,
        1px 6px 1px #919191,
        1px 7px 1px #919191,
        // 1px 8px 1px #919191,
        // 1px 9px 1px #919191,
        // 1px 10px 1px #919191,
    1px 18px 6px rgba(16,16,16,0.4),
    1px 22px 10px rgba(16,16,16,0.2),
    1px 25px 35px rgba(16,16,16,0.2),
    1px 30px 60px rgba(16,16,16,0.4);
}



// {
//   font-family:Georgia, 'Times New Roman', Times, serif;
//   font-size: 48px;
//   color:#F25360;
//   font-weight: bolder;
// }
</style>

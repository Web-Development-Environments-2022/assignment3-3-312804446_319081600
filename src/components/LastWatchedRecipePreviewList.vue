<template>
  <b-container>
    <h3>
      {{ title }}:
      <slot></slot>
    </h3>
    <b-row>
      <b-col v-for="r in recipes" :key="r.id">
        <LastWatchedRecipePreview class="LastWatchedRecipePreview" :recipe="r" />
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import LastWatchedRecipePreview from "./LastWatchedRecipePreview.vue";
export default {
  name: "LastWatchedRecipePreviewList",
  components: {
    LastWatchedRecipePreview
  },
  props: {
    title: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      recipes: []
    };
  },
  mounted() {
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {
      try {
        const response = await this.axios.get(
          this.$root.store.server_domain + "/users/lastWatched", 
          // "https://test-for-3-2.herokuapp.com/recipes/random"
        );

        // console.log(response);
        const recipes = response.data.results;
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

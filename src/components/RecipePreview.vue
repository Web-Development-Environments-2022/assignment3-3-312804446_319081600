<template>
  <router-link
    :to="{ name: 'recipe', params: { recipeId: recipe.id, route_name: route_name} }"
    class="recipe-preview"
  >

  <div class="recipe-body">
      <b-card no-body class="overflow-hidden" style="max-width: 540px;">
        <b-row no-gutters>
          <b-col md="6">
            <b-card-img :src= "recipe.image" alt="Image" class="rounded-0"></b-card-img>
          </b-col>
          <b-col md="6">
            <b-card-body :title= "recipe.title">
              <b-card-text>
                <b-list-group align="center">
                  <dt> Ready in {{ recipe.readyInMinutes }} minutes</dt>
                  <dt>  {{ recipe.aggregateLikes }} likes</dt>
                </b-list-group>
                <!-- <input
                    v-show="recipe.vegetarian"
                    type="image"
                    style=" margin: 0px 5px;"
                    src="https://icon-library.com/images/vegan-icon/vegan-icon-13.jpg"
                  />
                  <input
                  v-show="!recipe.vegetarian"
                  type="image"
                  style=" margin: 0px 5px;"
                  src="https://icon-library.com/images/vegan-icon/vegan-icon-13.jpg"
                /> -->
                  <ul class="recipe-overview">
                    <li>{{ recipe.vegan }} vagen</li>
                    <li>{{ recipe.vegetarian }} vageterian</li>
                    <li>{{ recipe.glutenFree }} Gluten Free</li>

                      
                  </ul>
              </b-card-text>
            </b-card-body>
          </b-col>
        </b-row>
      </b-card>
    </div>
  </router-link>
</template>

<script>
export default {
  // mounted() {
  //   this.axios.get(this.recipe.image).then((i) => {
  //     this.image_load = true;
  //   });
  // },
  data() {
    return {
      // image_load: false
    };
  },
  props: {
    recipe: {
      type: Object,
      required: true
    },

    // id: {
    //   type: Number,
    //   required: true
    // },
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
.recipe-preview {
  display: inline-block;
  width: 90%;
  height: 100%;
  position: relative;
  margin: 10px 10px;
}
.recipe-preview > .recipe-body {
  width: 100%;
  height: 200px;
  position: relative;
}

.recipe-preview .recipe-body .recipe-image {
  margin-left: auto;
  margin-right: auto;
  margin-top: auto;
  margin-bottom: auto;
  display: block;
  width: 98%;
  height: auto;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  background-size: cover;
}

.recipe-preview .recipe-footer {
  width: 100%;
  height: 50%;
  overflow: hidden;
}

.recipe-preview .recipe-footer .recipe-title {
  padding: 10px 10px;
  width: 100%;
  font-size: 12pt;
  text-align: left;
  white-space: nowrap;
  overflow: hidden;
  -o-text-overflow: ellipsis;
  text-overflow: ellipsis;
}

.recipe-preview .recipe-footer ul.recipe-overview {
  padding: 5px 10px;
  width: 100%;
  display: -webkit-box;
  display: -moz-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  box-flex: 1;
  -webkit-flex: 1 auto;
  -ms-flex: 1 auto;
  flex: 1 auto;
  table-layout: fixed;
  margin-bottom: 0px;
}

.recipe-preview .recipe-footer ul.recipe-overview li {
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  -ms-box-flex: 1;
  box-flex: 1;
  -webkit-flex-grow: 1;
  flex-grow: 1;
  width: 90px;
  display: table-cell;
  text-align: center;
}
</style>

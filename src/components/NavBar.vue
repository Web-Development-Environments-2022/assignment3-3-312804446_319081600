<template>
  <div id="app">
    <div id="nav">
      <router-link :to="{ name: 'main' }">Vue Recipes</router-link>|
      <router-link :to="{ name: 'search' }">Search</router-link>|
      <!-- this is the false close to the username -->
      {{ !$root.store.username }}
      <span v-if="!$root.store.username">
        Guest:
        <router-link :to="{ name: 'register' }">Register</router-link>|
        <router-link :to="{ name: 'login' }">Login</router-link>|
      </span>
      <span v-else>
        <select name="format" id="format" v-on:change="changeRoute($event)">
          <option ></option>
          <option value="favorites" >favorites</option>
          <option value="MyRecipes">MyRecipes</option>
          <option value="familyRecipes">familyRecipes</option>
        </select>
        <span>
          <b-button v-b-modal.modal-1>Create Model</b-button>
          <!-- <b-button id="show-btn" @click="$bvModal.show('modal-1')">Open Modal</b-button> -->
          <b-modal id="modal-1" ref="my-modal" title="Create Recipe" hide-footer>
            <b-form @submit.prevent="onCreate" @reset.prevent="onResetCreate" >
              <b-form-group>
                <b-form-input
                  id="input-1"
                  v-model="form.recipename"
                  type="text"
                  placeholder="Enter recipe name"
                  required
                ></b-form-input>
              </b-form-group> 

              <b-form-group>
                <b-form-input
                  id="input-2"
                  v-model="form.servings"
                  type="number"
                  placeholder="Enter servings number"
                  required
                ></b-form-input>
              </b-form-group> 

              <b-form-group>
                <b-form-input
                  id="input-3"
                  v-model="form.readyInMinutes"
                  type="number"
                  placeholder="Enter preparation time in minutes "
                  required
                ></b-form-input>
              </b-form-group> 

              <b-form-group>
                <b-form-input
                  id="input-4"
                  v-model="form.image"
                  type="url"
                  placeholder="Enter the URL image of the recipe "
                  required
                ></b-form-input>
              </b-form-group>

             <b-form>
              <b-form-group>
                <b-form-input v-model="form2.ingredient" type="text" required placeholder="Enter ingredient"></b-form-input>
              </b-form-group>

              <b-form-group>
                <b-form-input v-model="form2.amount" type="number" required placeholder="Enter amount"></b-form-input>
              </b-form-group>

                <button @click="add_ingredient">ADD</button>
                <p>ingredients:
                {{this.display_ingredients}}
                </p>
              </b-form>


              <b-form-textarea
                id="textarea"
                v-model="form.insrtaction"
                placeholder="Enter insrtaction of recipe..."
                rows="4"
                required
              ></b-form-textarea>
            
            <b-form-checkbox v-model="form.checked_gluten" name="check-button1" switch>
              gluten free 
            </b-form-checkbox>       
            <b-form-checkbox v-model="form.checked_vegan" name="check-button2" switch>
              vegan 
            </b-form-checkbox>
            <b-form-checkbox v-model="form.checked_vegetarian" name="check-button3" switch>
              vegetarian 
            </b-form-checkbox>                  
            
            <b-button type="reset" variant="danger">Cancel</b-button>
            <!-- <b-button @click="Register" -->
            <b-button
              type="submit"
              variant="primary"
              style="width:250px;"
              class="ml-5 w-75"
              >Create</b-button>
            </b-form>            
          </b-modal>
        </span>
        {{ $root.store.username }}: <button @click="Logout">Logout</button>|
      </span>
    </div>
    <router-view />
  </div>
</template>


<script>
// import CreateRecipe from './CreateRecipe.vue';
export default {
  data() {
    return {
      form: {
        recipename: '',
        servings: '',
        readyInMinutes:'',
        image:'',
        checked_gluten: false,
        checked_vegetarian: false,
        checked_vegan: false,
        insrtaction:'',

        },
      form2: {
        ingredient:'',
        amount:''
      },
        ingredients:[],
        display_ingredients:""
      }
    },
  methods: {
    changeRoute(e) {
      this.$router.push("/users/" + e.target.value);
    },
    Logout() {
      this.$root.store.logout();
      this.$root.toast("Logout", "User logged out successfully", "success");
      this.$router.push("/").catch(() => {
        this.$forceUpdate();
      });
    },

    async Create() {
      try {
        console.log("chack")
        const user_id = await this.axios.get(
          this.$root.store.server_domain + "/users/userid"
        );
        console.log(user_id.data)
        const response = await this.axios.post(
          // "https://test-for-3-2.herokuapp.com/user/Register",
          this.$root.store.server_domain + "/users/CreateRecipe",
        {
          user_id: user_id.data,
          image: this.form.image,
          title: this.form.recipename,
          readyInMinutes: this.form.readyInMinutes ,
          glutenFree: this.form.checked_gluten & 1,
          aggregateLikes: 0,
          instructions: this.form.insrtaction,
          servings: this.form.servings,
          vegan: this.form.checked_vegan & 1,
          vegetarian: this.form.checked_vegetarian & 1,
          
          // ingredients: req.body.ingredients -> "ingredients": [{"name": ,"amount":},{"name": ,"amount":}...]
          ingredients: this.ingredients
        }
        );
        console.log(response)

        await this.onResetCreate();
        this.$refs['my-modal'].hide()
        // moves to login rout right after registration 
        // console.log(response);
      } catch (err) {
        console.log(err.response);
        this.form.submitError = err.response.data.message;
      }
    },
    onCreate(){   
      if(this.ingredients.length > 0){
        this.Create()
        this.$root.toast("Create", "Recipes created successfully", "success");
      }     
      else{
        this.$root.toast("error", "there is no ingredients", "error");
      }
      
    },
    async onResetCreate(){
     
      this.form={
        recipename: '',
        servings: '',
        readyInMinutes:'',
        image:'',
        checked_gluten: false,
        checked_vegetarian: false,
        checked_vegan: false,
        insrtaction:'',

      },
      this.form2={
        ingredient:'',
        amount:''
      }
      this.ingredients=[],
      this.display_ingredients=""
      
    },
    add_ingredient(){
      if(this.form2.ingredient !== "" && this.form2.amount !=="")
      {      
        let obj = {
        "name": this.form2.ingredient,
        "amount": this.form2.amount
        }    
      this.ingredients.push(obj);
      console.log(this.form2.ingredient + ":" + this.form2.amount)
      this.display_ingredients = this.display_ingredients + this.form2.ingredient + ":" + this.form2.amount + ", "
      // this.display_ingredients.push(this.form2.ingredient + ":" + this.form2.amount)
      // const myJSON = JSON.stringify(this.ingredients);
      // document.getElementById("ing") = myJSON
      this.reset_ingredient();
      console.log(obj);}
    },
    reset_ingredient(){
      this.form2={
        ingredient:'',
        amount:''
      }
    }

  },
};
</script>

<style>
#nav {
  padding-top: 10px;
  padding-bottom: 10px;
  margin-left: 0 !important;
  height: 62px;
  width: 100%;
  background-color: whitesmoke;
  font-family: 'Palanquin Dark';
  justify-content: center;
  font-size: 22px;
  -webkit-box-shadow: -2px 2px 9px 7px rgba(0,0,0,0.53);
  -moz-box-shadow: -2px 2px 9px 7px rgba(0,0,0,0.53);
  box-shadow: -2px 2px 9px 7px rgba(0,0,0,0.53);
  z-index: 10;
}
#nav a {
  color: #333333;
  padding: 10px;
  margin: 0;
}
#nav a:hover {
  opacity: 70%;
  text-decoration: none;
}
#nav a.router-link-exact-active {
  opacity: 70%;
}
#my-logo{
  padding: 10px;
  padding-bottom: 10px;
  margin-right: 20px;
  margin-left: -15px !important;
  padding-left: 35px;
  padding-right: 25px;
  font-size: 120%;
  background-color: #333333;
  color: whitesmoke !important;
}
#my-logo:hover{
  text-decoration: none;
  opacity: 90%; 
}
#my-logo.router-link-exact-active{
  text-decoration: none;
  opacity: 100% !important;
}
.sticky {
  position: fixed;
  top: 0;
  width: 100%;
}
#user-section{
  text-align: right;
  justify-content: center;
  color: #333333;
  padding-bottom: 3px;
}
#main-section a{
  margin-left: 20px;
}
.user-section{
  margin-left: 30px;
  font-size: 22px;
}
#user-section{
  display: flex;
  flex-direction: column;
  justify-content: center;
}
#logout-btn,
#logout-btn:focus
{
  outline: none;
  border: none;
}
#logout-btn:hover {
  opacity: 70%;
}
.hello {
  padding-right: 25px;
}
#personal {
  font-family: 'Palanquin Dark';
}
#personal .dropdown-menu {
  background-color: whitesmoke;
  padding: 3px;
  border: none;
}
#personal a {
  margin-left: 0;
  font-size: 20px !important;
}
#personal button::after {
  display: none;
}
#personal button:focus,
#personal button {
  background-color: transparent;
  color: #333333;
  margin-left: 20px;
  outline: none;
  border: none;
  box-shadow: none;
  font-size: 22px;
  padding: 10px;
  padding-top: 4px;
}
</style>

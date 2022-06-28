<template>
  <div>
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
    <b-row>
        <b-col lg="3" class="pb-2"><b-button type="submit" pill variant="success" >Create</b-button></b-col>
        <b-col lg="3" class="pb-2"><b-button type="reset" pill variant="danger">Cancel</b-button></b-col>
        
    </b-row>
    <!-- <b-button @click="Register" -->
    </b-form>            
    </b-modal>    
  </div>
</template>

<script>
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
  }

}
</script>

<style>

</style>
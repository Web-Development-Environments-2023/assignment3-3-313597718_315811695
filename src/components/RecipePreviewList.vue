<template>
  <b-container>
    <b-row>
      <b-col v-for="r in recipes" :key="r.id">
        <span v-if="state === 'Family'">
          <FamilyRecpieComponent
            class="recipePreview"
            :recipe="r"
            :state="state"
          />
        </span>
        <span v-else>
          <RecipePreview class="recipePreview" :recipe="r" :state="state" />
        </span>
      </b-col>
    </b-row>
    <br><h3 v-if="noResult">No Result found, try another search.</h3>
  </b-container>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";

import FamilyRecpieComponent from "./familyRecpieComponent.vue";
export default {
  name: "RecipePreviewList",
  components: {
    RecipePreview,
    FamilyRecpieComponent,
  },
  props: {
    sortBy: {
      tpye: String,
      required: false,
    },
    title: {
      type: String,
      required: true,
    },
    state: {
      type: String,
      required: true,
    },
    recipeskeywords: {
      type: String,
      required: false,
    },
    cuisine: {
      type: String,
      required: false,
    },
    intolerances: {
      type: String,
      required: false,
    },
    diet: {
      type: String,
      required: false,
    },
    num: {
      type: String,
      required: false,
    },
  },
  data() {
    return {
      recipes: [],
      noResult: false,
    };
  },
  mounted() {
    //console.log("mouunted state is :", this.state);
    switch (this.state) {
      case "random":
        this.randomRecipes();
        break;
      case "last3":
        this.lastRecipes();
        break;
      case "search":
        this.onSearch();
        break;
      case "favorites":
        this.onFavorites();
        break;
      case "MyRecipes":
        // console.log("in my recieps switch case");
        this.onMyRecipes();
        break;
      case "Family":
        this.onFamily();
        break;
      default:
        // console.log(" switch case defult -------->", this.state);
    }
  },
  methods: {
    async onFavorites() {
      try {
        const response = await this.axios.get(
          "https://bgfood.cs.bgu.ac.il/users/getfavorites",
          { withCredentials: true }
          //this.$root.store.server_domain + "/recipes/random",
          // "https://test-for-3-2.herokuapp.com/recipes/random"
        );

        // console.log(response);
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
        // console.log(this.recipes);
      } catch (error) {
        console.log("Error: " + error);
      }
    },

    async onSearch() {
      try {
        // console.log("this.recipeskeywords:",this.recipeskeywords);
        // console.log("on search; sort = ", this.sortBy);
        if (this.recipeskeywords == "") {
          // console.log("query is null");
          return;
        }
        const response = await this.axios.get(
          `https://bgfood.cs.bgu.ac.il/recipes/search?recipeskeywords=${this.recipeskeywords}&num=${this.num}&cuisine=${this.cuisine}&intolerances=${this.intolerances}&diet=${this.diet}`,
          { withCredentials: true }
        );

        // console.log("on search , respones : ", response);
        if (typeof response.data == "undefined") {
          this.noResult = true;
          return;
        }
        const recipes = response.data;
        if (recipes.length == 0) {
          // console.log("answer is empty");
          this.noResult = true;
          return;
        }
        // this.$root.loggedIn = true;

        this.recipes = [];
        this.recipes.push(...recipes);

        // console.log("on search , sort = ", this.sortBy);
        switch (this.sortBy) {
          case "readyInMinutes":
            //console.log("in readyInMinutes ");
            this.recipes.sort(function(a, b) {
              return a.readyInMinutes - b.readyInMinutes;
            });
            break;
          case "aggregateLikes":
            this.recipes.sort(function(a, b) {
              return b.aggregateLikes - a.aggregateLikes;
            });
            break;
          default:
        }
        //console.log("after switch case");
      } catch (err) {
        console.log("Error: " + err.response);
        //this.form.submitError = err.response.data.message;
      }
    },
    async onMyRecipes() {
     // console.log("in My Recipes in RecipePreviewList");
      try {
        const response = await this.axios.get(
          "https://bgfood.cs.bgu.ac.il/users/getMyrecipes"
        );
        //console.log("response12: ", response);
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
        //console.log("answer of MyRecpies: ", this.recipes);
      } catch (error) {
        console.log("error: ", error.response.data);
      }
    },

    async randomRecipes() {
      try {
        const response = await this.axios.get(
          "https://bgfood.cs.bgu.ac.il/recipes/random",
          { withCredentials: true }
          //this.$root.store.server_domain + "/recipes/random",
          //"https://test-for-3-2.herokuapp.com/recipes/random"
        );

        //console.log("respone in preview list ", response);
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
        //console.log(this.recipes);
      } catch (error) {
        console.log("Error: " + error.response.data);
      }
    },
    async onFamily() {
      //console.log("on Family");
      try {
        const response = await this.axios.get(
          "https://bgfood.cs.bgu.ac.il/users/getFamily",
          { withCredentials: true }
        );
       // console.log("family result: ", response.data);
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
        //console.log("recpies are (194) ", this.recipes);
      } catch (err) {
        console.log("Error: " +  err);
      }
    },
    async lastRecipes() {
      try {
        const response = await this.axios.get(
          "https://bgfood.cs.bgu.ac.il/recipes/getLast3",
          { withCredentials: true }
          //this.$root.store.server_domain + "/recipes/random",
          // "https://test-for-3-2.herokuapp.com/recipes/random"
        );

        //console.log("last 3 response: ", response);
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
        //console.log(this.recipes);
      } catch (error) {
        console.log("Error: " + error);
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}
</style>

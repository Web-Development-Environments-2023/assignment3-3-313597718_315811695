<template>
  <div class="container">
    <br><h1 class="title">Search</h1>
    <div >
    <b-form v-on:submit.prevent="onSearch"  >
      <b-form-group id="input-group-search" label-for="search" >
        <b-form-input
          id="search"
          v-model="form.recipeskeywords"
          placeholder="Enter your query here"
          type="search"
          required
         class="searcarea"></b-form-input>
        <b-button type="submit" variant="primary" class="searcarea">Submit</b-button>
      </b-form-group>
    </b-form>
    </div>
    <div class="searchitems">
      Cuisine:
      <select v-model="cuisine">
        <option
          v-for="(c, index) in cuisineArray"
          :key="index"
          :value="c.value"
        >
          {{ c.text }}
        </option>
      </select>

      Intolerances:
      <select v-model="intolerances">
        <option
          v-for="(i, index) in intolerancesArray"
          :key="index"
          :value="i.value"
        >
          {{ i.text }}
        </option>
      </select>

      Diet:
      <select v-model="diet">
        <option v-for="(d, index) in dietArray" :key="index" :value="d.value">
          {{ d.text }}
        </option>
      </select>

      Number of result:
      <select v-model="num">
        <option v-for="(n, index) in numArray" :key="index" :value="n.value">
          {{ n.text }}
        </option>
      </select>

       Sort by:
      <select v-model="sortBy">
        <option v-for="(s, index) in sortByArray" :key="index" :value="s.value">
          {{ s.text }}
        </option>
      </select>
    </div><br>
    <div v-if="lastSearch != ''">Your last serach was : {{ lastSearch }}</div>
    <RecipePreviewList
      :key="componentKey"
      v-if="componentKey > 0"
      title="Search Result"
      state="search"
      :sortBy="this.sortBy"
      :recipeskeywords="this.recipeskeywords"
      :cuisine="this.cuisine"
      :intolerances="this.intolerances"
      :diet="this.diet"
      :num="this.num"
      class="RandomRecipes center"
    />
  </div>
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList";

export default {
  components: {
    RecipePreviewList,
  },
  data() {
    return {
      componentKey: 1,
      recipeskeywords: "",
      cuisine: "",
      cuisineArray: [
        { text: "None", value: "" },
        { text: "African", value: "African" },
        { text: "American", value: "American" },
        { text: "British", value: "British" },
        { text: "Cajun", value: "Cajun" },
        { text: "Caribbean", value: "Caribbean" },
        { text: "Chinese", value: "Chinese" },
        { text: "Eastern European", value: "Eastern European" },
        { text: "European", value: "European" },
        { text: "French", value: "French" },
        { text: "German", value: "German" },
        { text: "Greek", value: "Greek" },
        { text: "Indian", value: "Indian" },
        { text: "Irish", value: "Irish" },
        { text: "Italian", value: "Italian" },
        { text: "Japanese", value: "Japanese" },
        { text: "Jewish", value: "Jewish" },
        { text: "Korean", value: "Korean" },
        { text: "Latin American", value: "Latin American" },
        { text: "Mediterranean", value: "Mediterranean" },
        { text: "Mexican", value: "Mexican" },
        { text: "Middle Eastern", value: "Middle Eastern" },
        { text: "Nordic", value: "Nordic" },
        { text: "Southern", value: "Southern" },
        { text: "Spanish", value: "Spanish" },
        { text: "Thai", value: "Thai" },
        { text: "Vietnamese", value: "Vietnamese" },
      ],
      intolerancesArray: [
        { text: "None", value: "" },
        { text: "Dairy", value: "Dairy" },
        { text: "Egg", value: "Egg" },
        { text: "Gluten", value: "Gluten" },
        { text: "Grain", value: "Grain" },
        { text: "Peanut", value: "Peanut" },
        { text: "Seafood", value: "Seafood" },
        { text: "Sesame", value: "Sesame" },
        { text: "Shellfish", value: "Shellfish" },
        { text: "Soy", value: "Soy" },
        { text: "Sulfite", value: "Sulfite" },
        { text: "Tree Nut", value: "Tree Nut" },
        { text: "Wheat", value: "Wheat" },
      ],
      dietArray: [
        { text: "None", value: "" },
        { text: "Gluten Free", value: "Gluten Free" },
        { text: "Ketogenic", value: "Ketogenic" },
        { text: "Vegetarian", value: "Vegetarian" },
        { text: "Lacto-Vegetarian", value: "Lacto-Vegetarian" },
        { text: "Ovo-Vegetarian", value: "Ovo-Vegetarian" },
        { text: "Vegan", value: "Vegan" },
        { text: "Pescetarian", value: "Pescetarian" },
        { text: "Paleo", value: "Paleo" },
        { text: "Primal", value: "Primal" },
        { text: "Low FODMAP", value: "Low FODMAP" },
        { text: "Whole30", value: "Whole30" },
      ],
      num: "5",
      sortBy: "None",
      sortByArray: [
        { text: "None", value: "None" },
        { text: "Likes", value: "aggregateLikes" },
        { text: "Minutes", value: "readyInMinutes" },//new
      ],
      numArray: [
        { text: "5", value: "5" },
        { text: "10", value: "10" },
        { text: "15", value: "15" },
      ],
      intolerances: "",
      diet: "",

      form: {},
      clicked: false,
      lastSearch: "",
    };
  },
  mounted() {
    this.lastSearch = "";
    this.getLastSearch();
  },

  methods: {
    async onSearch() {
      this.getLastSearch();
      this.componentKey += 1;

      // if(this.componentKey!=0){
      // this.lastSearch = this.form.recipeskeywords;
      // }

      this.recipeskeywords = this.form.recipeskeywords;
      //console.log("sumbit click with ", this.form.recipeskeywords);

      // this.clicked = true;
    },
    async getLastSearch() {
      try {
        //console.log("on get last search");
        const response = await this.axios.get(
          "https://bgfood.cs.bgu.ac.il/users/lastSearch",
          { withCredentials: true }
        );
        this.lastSearch = response.data;
        //console.log("getLastSearch respones is : ", response.data);
      } catch (error) {}
    },
  },
};
</script>

<style>
.searcarea{
  max-width: 70%;
  margin-right: 10px;
  display: inline;
}
.searchitems{
  max-width: 120%;

}
</style>

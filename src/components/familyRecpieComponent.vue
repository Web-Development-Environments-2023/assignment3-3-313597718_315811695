<template>
  <div>
    <b-card no-body>
      <b-card :title="recipe.title" :img-src="recipe.image" />

      <b-card-body>
        <b-card-text>
          <!-- state is :{{ state }} recpies is : {{ recipe.id }} or {{ recipe.id }} -->
          <ul class="recipe-overview">
            <li>Created by {{ recipe.owner }}</li>
            <li>When to make:{{ recipe.whentomake }}</li>
            <li>Ingredients: {{ recipe.ingredients }}</li>
            <li>Instructions: {{ recipe.instructions }}</li>
          </ul>
        </b-card-text>
      </b-card-body>
    </b-card>
  </div>
</template>
<script>
export default {
  mounted() {
    // console.log("in famliy");
    // this.axios.get(this.recipe.image).then((i) => {
    //   this.image_load = true;
    // });
  },
  data() {
    return {
      image_load: false,
    };
  },

  props: {
    recipe: {
      type: Object,
      required: true,
    },
    state: {
      tpye: String,
      required: false,
    },
    //
    //
  },
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

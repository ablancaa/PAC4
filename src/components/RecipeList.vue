<template>
  <div id="recipe-list" class="recipe-list">
    <div v-for="recipe in recipeList" :key="recipe.id">
      <recipe :recipe="recipe" @deleteRecipe="deleteRecipe"/>
    </div>
  </div>
</template>

<script>
import { defineComponent } from "vue";
import Recipe from "./Recipe.vue";

export default defineComponent({
  name: "RecipeList",
  props: {
    recipeList: {
      type: Array,
      required: true,
    }
    },
    components: { Recipe },
    
    data() {
        return {
      }
    },
    computed: {},
    methods: {
    /* Esdeveniment encarregat d'informar que s'ha eliminat una recepta amb
       identificador id.*/
    /* Esdeveniment encarregat d'informar que s'ha eliminat una recepta.
       Aquest esdeveniment es propagarà quan es capturi l'esdeveniment delete-recipe per part
       del component RecipeCard.vue.*/
      deleteRecipe(info){
        this.$emit('deleteRecipe', info);
        console.log("Desde recipeList: "+ info);
      },

    }//FIN METHODS

});//FIN EXPORT DEFAULT
</script>

<style scoped>
.recipe-list {
  display: grid;
  width: 100%;
  grid-template-columns: 1fr 1fr 1fr;
}
</style>

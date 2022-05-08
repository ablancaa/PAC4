<template>
  <div id="recipe-list" class="recipe-list">
    <div v-for="recipe in recipeList" :key="recipe.id">
      <recipe :recipe="recipe" @deleteRecipe="deleteRecipe"/>
    </div>
  </div>
</template>

<script>
import { defineComponent, } from "vue";
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
  emits:['deleteRecipe'],
  
  setup(props, context){

    /* Esdeveniment encarregat d'informar que s'ha eliminat una recepta amb
       identificador id.*/
    /* Esdeveniment encarregat d'informar que s'ha eliminat una recepta.
       Aquest esdeveniment es propagarà quan es capturi l'esdeveniment delete-recipe per part
       del component RecipeCard.vue.*/

       const deleteRecipe = (id) => {
         context.emit('deleteRecipe', id);
         console.log("FUNCIÓN deleteRecipe() desde recipeList emite => "+id);
       }

      return { deleteRecipe }

  },//FIN setup()

});//FIN EXPORT DEFAULT
</script>

<style scoped>
.recipe-list {
  display: grid;
  width: 100%;
  grid-template-columns: 1fr 1fr 1fr;
}
</style>

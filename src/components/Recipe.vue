<template>
  <div class="recipe" :class="recipe.featured && 'featured'">
    <div class="estrella-recipe" v-show="recipe.featured"><img src="@/assets/estrella.png" /></div>
    <button class="delete-recipe" @click="deleteRecipe">
      <img src="../assets/delete-button.svg" alt="Delete recipe" />
    </button>
    <h2 class="recipe-title">{{ recipe.title }}</h2>
    <div class="recipe-image">
      <img :src="recipe.imageUrl" />
    </div>
    <div class="recipe-info">
      <div class="recipe-info-item">
        <span class="recipe-info-label">Servings:</span>
        <span class="recipe-info-value">{{ recipe.servings }}</span>
      </div>
      <div class="recipe-info-item">
        <span class="recipe-info-label">Time:</span>
        <span class="recipe-info-value">{{ recipe.time }}</span>
      </div>
      <div class="recipe-info-item">
        <span class="recipe-info-label">Difficulty:</span>
        <span class="recipe-info-value">{{ recipe.difficulty }}</span>
      </div>
    </div>
    <div class="recipe-ingredients">
      <h3 class="recipe-ingredients-title">Ingredients</h3>
      <ul class="recipe-ingredients-list">
        <li v-for="ingredient in recipe.ingredients" :key="ingredient">
          {{ ingredient }}
        </li>
      </ul>
    </div>
    <div class="recipe-directions">
      <h3 class="recipe-directions-title">Directions</h3>
      <ol class="recipe-directions-list">
        <li v-for="direction in recipe.directions" :key="direction">
          {{ direction }}
        </li>
      </ol>
    </div>
  </div>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "RecipeCard",
  props: {
    recipe: {
      type: Object,
      required: true,
    },
  },
  methods: {
    /* Aquest mètode s'ha d'executar cada vegada que es fes clic al botó amb la X. 
      Haureu d'emetre els esdeveniments següents: 
        ○ delete-recipe(id): Esdeveniment encarregat d'informar que s'ha eliminat 
          una recepta. Indica l'identificador id de la recepta com a paràmetre.*/
    deleteRecipe () {
      this.$emit("deleteRecipe", this.recipe.id);
      console.log("Desde Recipe: "+this.recipe.id);
    } 
  },//FIN METHODS
});
</script>

<style scoped>
.recipe {
  padding: 20px;
  border: 1px solid #ccc;
  margin: 20px;
  border-radius: 4px;
  display: block;
  position: relative;
}
.recipe.featured {
  background-color: #f5f5f5;
}
.recipe-title {
  font-weight: bold;
  font-size: 28px;
}
.recipe-image {
  margin-bottom: 20px;
  max-height: 160px;
  overflow: hidden;
}
.recipe-image img {
  width: 100%;
}
.recipe-info {
  margin-bottom: 20px;
  display: flex;
  justify-content: space-between;
}
.recipe-info-label {
  font-weight: bold;
  margin-right: 5px;
}
.recipe-ingredients,
.recipe-directions {
  margin-bottom: 20px;
  text-align: left;
}
.recipe-ingredients-title,
.recipe-directions-title {
  font-weight: bold;
  margin-bottom: 10px;
}
.recipe-ingredients-list,
.recipe-directions-list {
  padding: 0;
  margin: 0;
  margin-left: 18px;
}
.delete-recipe {
  position: absolute;
  top: 10px;
  right: 10px;
  background: transparent;
  border: 0;
  padding: 0;
  cursor: pointer;
}
.delete-recipe img {
  width: 20px;
}
.estrella-recipe {
position: absolute;
top: -30px;
left:-32px;
background: transparent;
border: 0;
padding: 0;
cursor: pointer;
}
.estrella-recipe img {
width: 90px;
}
</style>

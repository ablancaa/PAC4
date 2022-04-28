<template>
  <div id="app">
    <div class="header">
      <img class="logo" alt="UOC logo" src="./assets/uoc-logo.png" />
      <div class="app-name">Recipe book</div>
    </div>
    <search-bar @openForm="toggleForm" @newVal="setSearchTerm" @clearSearch="setSearchTerm"/>
    <recipe-list :recipeList="recipeListFiltered" @deleteRecipe="deleteRecipe"/>
    <recipe-form v-if="showModal" @closeForm="toggleForm"  @nuevaReceta="addRecipe"/>
    
  </div>
</template>

<script>
import RecipeList from "./components/RecipeList.vue";
import RecipeForm from "./components/RecipeForm.vue";
import SearchBar from "./components/SearchBar.vue";
import axios from "axios";
//import CompositionApi from '@vue/composition-api';
import { defineComponent } from "vue";

export default defineComponent({
  name: "App",
  components: {
    RecipeList: RecipeList,
    RecipeForm,
    SearchBar,
  },
  data: () => ({
    recipeList: [],
    showModal: false,
    searchTerm:'',
    filterData: [],  
  }),
  setup(){},
  async created(){
    try {
    axios
    .get("http://localhost:3000/recipes")
    .then((recipe) => {
      this.recipeList = recipe.data.recipes;
      console.log(this.recipeList);
    })
    } catch (error) {
      console.log(error);
    }
    
  },
  computed: {
    
  /* Funció que:
      ○ Retorna el llistat de receptes en el cas que searchTerm estigui buit.
      ○ Retorna la col·lecció de receptes filtrada pels termes de cerca. Heu de buscar si
        searchTerms forma part del nom de la recepta o dels ingredients a cada recepta. */
    recipeListFiltered() {

      let listaFiltrada = [];
      
      if(this.searchTerm != ''){
        listaFiltrada = this.recipeList.filter(recipe => 
          recipe.title.toLowerCase().includes(this.searchTerm.toLowerCase()) ||
          recipe.ingredients.includes(this.searchTerm.toLowerCase()),
        )
      } else {
        listaFiltrada = this.recipeList;
      }
      return listaFiltrada;
    },
    
  },     
  methods: {
    /*getRecetas(){
      axios
         .get("http://localhost:3000/recipes")
         .then((res) => {
           this.recipeList = res.data.recipes;
           console.log(this.recipeList);
         })
         .catch((error) => {
           console.log(error);
         });
         
    },*/
  /* Afegeix un objecte de tipus Recipe a l'array d'elements recipeList. */
    addRecipe(recipe){
      this.recipeList.push(recipe);
      console.log("receta añadida: "+recipe);
    },

  /*Elimina l'objecte de la llista recipeList l'identificador id és el
  passat per paràmetre.*/
    deleteRecipe(recipeId){
      let busqueda = recipeId;
      console.log("Tenemos el array de recetas: ", this.recipeList);
      console.log("Buscando en donde el ID de la receta sea igual a: ", busqueda);
      let indice = this.recipeList.findIndex(receta => receta.id === busqueda);
      console.log("La receta buscada está en el índice ", indice);
      //Elimina la receta con el id seleccionado
      this.recipeList.splice(indice, 1);
    },

  /*Modifica l'estat del paràmetre showModal al seu invers.*/
    toggleForm(info){
      if (info == true){
        this.showModal = true;
      } else {
        this.showModal = false;
      }
    },
    
  /*Actualitza un paràmetre searchTerm (de nova creació al component) amb
  la informació rebuda a l'esdeveniment.*/
    setSearchTerm(info){
      this.searchTerm = info;
      if (this.searchTerm == ''){
        this.recipeListFiltered;
      }
      console.log("setSearchInfo(): "+this.searchTerm);  
      //this.recipeListFiltered();   
    },
  },
  watch:{
    /*searchTerm: function(letra){
      this.searchTerm = this.searchTerm+letra;
      console.log("SearchTerm: "+this.searchTerm);
    }*/
  },
});
</script>

<style>
body {
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
  flex-wrap: wrap;
  height: 100%;
  width: 100%;
  max-width: 1280px;
  margin: 0 auto;
}
.header {
  width: 100%;
  padding: 15px;
  display: flex;
  align-items: center;
  border-bottom: 1px solid #ccc;
}
.header .logo {
  max-height: 50px;
}
.header .app-name {
  margin-left: 25px;
  font-weight: bold;
  font-size: 20px;
}
</style>
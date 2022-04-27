<template>

  <div class="modal-container">
    <div class="recipe-form">
      <div class="recipe-form-header">
        <h2>Add a new recipe</h2>
        <div id="errores">
            <p>{{ mensajeError }}</p>
        </div>
        <div id="add">
            <p>{{ mensajeError }}</p>
        </div>
        <button @click="closeForm">
          <img src="../assets/close-button.svg" alt="Close modal" />
        </button>
      </div>
      <form id="formulario" @submit.prevent="createRecipe">
        <div class="recipe-form-item">
          <label for="title">Title</label>
          <input type="text" id="title" v-model="title" />
        </div>
        <div class="recipe-form-item">
          <label for="imageUrl">Image URL</label>
          <input type="text" id="imageUrl" v-model="imageUrl"/>
        </div>
        <div class="recipe-form-item">
          <label for="servings">Servings</label>
          <input type="number" id="servings" v-model="servings"/>
        </div>
        <div class="recipe-form-item">
          <label for="time">Time</label>
          <input type="number" id="time" v-model="time"/>
        </div>
        <div class="recipe-form-item">
          <label for="difficulty">Difficulty</label>
          <select id="difficulty"  v-model="difficulty">
            <option value="Easy">Easy</option>
            <option value="Medium">Medium</option>
            <option value="Hard">Hard</option>
          </select>
        </div>
        <div class="recipe-form-item">
          <label for="ingredients">Ingredients</label>
          <textarea type="textarea" id="ingredients" v-model="ingredients"/>
        </div>
        <div class="recipe-form-item">
          <label for="directions">Directions</label>
          <textarea type="textarea" id="directions" v-model="directions"/>
        </div>
        <div class="recipe-form-item">
         <label for="checkbox">Featured</label><input type="checkbox" value="true" v-model="featured"/>
         <!-- <label>{{ featured }}</label> -->
         </div>
        <div class="recipe-form-item">
          <button type="submit" @submit.prevent="createRecipe">Add Recipe</button>
        </div>
      </form>
    </div>
  </div>

</template>

<script>
import { defineComponent } from "vue";
import { uuid } from 'vue-uuid'; 
export default defineComponent({
  name: "RecipeForm",
   data() {
            return {
                id: uuid.v1(),
                title: '',
                imageUrl: '',
                servings: '',
                time: '',
                difficulty: '',
                ingredients: [],
                directions: [],
                featured: '',
                submit: '',
                mensajeError: '',
                recipe:{},
            }
        },
        mounted(){},
        methods: {
            /*  Aquest mètode ha d'executar-se quan es faci el submit del formulari, és a
                dir, l'usuari faci click al botó Add Recipe, i s'encarregarà de:
                  ○ Comprovar que els camps title, ingredients i directions no estan buits. Si són buits
                    haureu de mostrar el missatge d'error prèviament descrit i no emetre la recepta.
                  ○ Separar el contingut dels camps ingredients i directions pel caràcter . (punt) i
                    afegir-los a un array.
                  ○ Crear un objecte (recipe) amb la informació guardada al formulari i els arrays
                    d'ingredients i directions..
                  ○ Emetre un esdeveniment add-recipe amb l'objecte creat.
                  ○ Esborrar els camps del formulari.  */
            createRecipe() {
                var error = document.getElementById("errores");
                var add = document.getElementById("add");
                
                //Valida que los campos no esten vacíos
                if(this.title == '' || this.ingredients == '' || this.directions == '') {

                //Chivatos de campos vacios
                  console.log("Titulo vacio!!");
                  console.log("Ingredientes Vacio!!");
                  console.log("Indicaciones Vacio!!");

                //Pinta en pantalla el error
                  error.innerHTML = 'The fields Title, Ingredients and Directions are required <br/>';
                  error.innerHTML += '<br/>';
                  add.innerHTML = '';

                } else {

                //Separo ingredientes e indicaciones si tienen un punto                        
                  let ingredienteUnaAuno = this.ingredients;
                  ingredienteUnaAuno = this.ingredients.split('.');
                  console.log("Ingredientes Separados: "+ingredienteUnaAuno);
                
                  let directionsUnaAuno = this.directions;
                  //directionsUnaAuno = this.directions.search('.');
                  directionsUnaAuno = this.directions.split('.');
                  //directionsUnaAuno = this.directions.split('\n');
                 
                  console.log("Directions Separados: "+directionsUnaAuno);
                /* let directionsUnaAuno = this.directions;
                  directionsUnaAuno = this.directions.replace(/\s/g, ' .');
                  console.log("Directions Separados: "+directionsUnaAuno);
                */
                //Indico si tiene el featured activado
                  if(this.featured==''){
                    this.featured= false;
                  } else {
                    this.featured= true;
                  }
                  
                //Creo el objeto receta para pasar a App 
                   let recipe = {
                        id: this.id, 
                        title: this.title, 
                        imageUrl: this.imageUrl, 
                        servings: this.servings,
                        time: this.time,
                        difficulty: this.difficulty,
                        ingredients: ingredienteUnaAuno,
                        directions: directionsUnaAuno,
                        featured: this.featured,
                    };

                //Emite la receta nueva a App
                    this.$emit('nuevaReceta', recipe);

                //Muestra datos de receta por consola
                    console.log("Emitida nueva receta: "+recipe);

                //Pongo el Div de error en blanco
                    error.innerHTML = '';

                //Indico en el Div add que se añade la receta nueva
                    add.innerHTML = 'Add recipe <br/>';
                    add.innerHTML += '<br/>';
                    
                //Datos introducidos mostrados por consola
                    console.log("//DATOS INTRODUCIDOS EN FORMULARIO")
                    console.log("Id: "+recipe.id);
                    console.log("Title: "+recipe.title);
                    console.log("imageURL: "+recipe.imageUrl);
                    console.log("Servings: "+recipe.servings);
                    console.log("Time: "+recipe.time);
                    console.log("Difficulty: "+recipe.difficulty);
                    console.log("Igredients: "+recipe.ingredients);
                    console.log("Directions: "+recipe.directions);
                    console.log("Featured: "+recipe.featured);
                    
                //Reinicio los campos
                    this.id = '';
                    this.title= '';
                    this.imageUrl = ''; 
                    this.servings = '';
                    this.time = '';
                    this.difficulty = '';
                    this.ingredients = '';
                    this.directions = '';
                    this.featured = '';
                    
                }//Fin if/else
                
            },//FIN createRecipe()

            /* Aquest mètode s'ha d'executar quan es faci clic al botó que conté el svg amb
               el símbol X. S'encarregarà de:
                ○ Emetre un esdeveniment close-modal  */
            closeForm() {
                this.$emit('closeForm', this.showModal = false);
              //Chivato para ver si emite
              //console.log("Función closeForm(){}: "+ this.showModal);
            }//FIN closeForm()
        },
});
</script>

<style scoped>
#errores { 
  color: red;
}
#add { 
  color: rgba(19, 199, 88, 0.781);
}
.modal-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  z-index: 10;
}
.recipe-form {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 80%;
  max-width: 500px;
  background: #fff;
  padding: 20px;
  border-radius: 5px;
}
/*.modal-container.recipe-form-header{
}*/
.recipe-form-header button {
  position: absolute;
  top: 10px;
  right: 10px;
  background: transparent;
  border: 0;
  padding: 0;
  cursor: pointer;
}
.recipe-form-header button img {
  width: 20px;
}
.recipe-form-item {
  margin-bottom: 20px;
}
.recipe-form-item label {
  display: block;
  font-weight: bold;
  margin-bottom: 5px;
}
.recipe-form-item input,
.recipe-form-item select,
.recipe-form-item textarea  {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  max-width: 300px;
}
.recipe-form-item button {
  background: #4caf50;
  color: #fff;
  padding: 10px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>

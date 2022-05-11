<template>

  <div class="modal-container">
    <div class="recipe-form">
      <div class="recipe-form-header">
        <h2>Add a new recipe</h2>
        <div id="errores">
            <p>{{  }}</p>
        </div>
        <div id="add">
            <p>{{  }}</p>
        </div>
        <button @click="closeForm">
          <img src="../assets/close-button.svg" alt="Close modal" />
        </button>
      </div>
      <form id="formulario" @submit.prevent="createRecipe">
        <div class="recipe-form-item">
          <label for="title">Title</label>
          <input type="text" id="title" v-model="recipe.title" />
        </div>
        <div class="recipe-form-item">
          <label for="imageUrl">Image URL</label>
          <input type="text" id="imageUrl" v-model="recipe.imageUrl"/>
        </div>
        <div class="recipe-form-item">
          <label for="servings">Servings</label>
          <input type="number" id="servings" v-model="recipe.servings"/>
        </div>
        <div class="recipe-form-item">
          <label for="time">Time</label>
          <input type="number" id="time" v-model="recipe.time"/>
        </div>
        <div class="recipe-form-item">
          <label for="difficulty">Difficulty</label>
          <select id="difficulty"  v-model="recipe.difficulty">
            <option value="Easy">Easy</option>
            <option value="Medium">Medium</option>
            <option value="Hard">Hard</option>
          </select>
        </div>
        <div class="recipe-form-item">
          <label for="ingredients">Ingredients</label>
          <textarea type="textarea" id="ingredients" v-model="recipe.ingredients"/>
        </div>
        <div class="recipe-form-item">
          <label for="directions">Directions</label>
          <textarea type="textarea" id="directions" v-model="recipe.directions"/>
        </div>
        <div class="recipe-form-item">
         <label for="checkbox">Featured</label><input type="checkbox" value="true" v-model="recipe.featured"/>
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
import { defineComponent, ref } from "vue";
import { uuid } from 'vue-uuid';
//import axios from "axios";
export default defineComponent({
  
  name: "RecipeForm",
  emits:['nuevaReceta','closeForm'],
    
    setup(props, context){
      let mensajeError = ref('The fields Title, Ingredients and Directions are required <br/>');
      let showModal = ref(false);
      let recipe = ref({
        id: uuid.v1(),
        title: '',
        imageUrl: '',
        servings: '',
        time: '', 
        difficulty: '',
        ingredients: [],
        directions: [],
        featured: ''
      });

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

        const createRecipe = () => {
          var error = document.getElementById("errores");
          var add = document.getElementById("add");
          //Valida que los campos no esten vacíos
          if(recipe.value.title == '' || recipe.value.ingredients == '' || recipe.value.directions == '') {
          //Chivatos de campos vacios
            console.log(recipe.value.id);
            console.log("Titulo vacio!!");
            console.log("Ingredientes Vacio!!");
            console.log("Indicaciones Vacio!!");
          //Pinta en pantalla el error
           // error.innerHTML = 'The fields Title, Ingredients and Directions are required <br/>';
            error.innerHTML = mensajeError.value;
            error.innerHTML += '<br/>';
            add.innerHTML = '';
          } else {
          //Separo ingredientes e indicaciones si tienen un punto                        
            let ingredienteUnaAuno = recipe.value.ingredients;
            ingredienteUnaAuno = recipe.value.ingredients.split('.');
            console.log("Ingredientes Separados: "+ingredienteUnaAuno);
            let directionsUnaAuno = recipe.value.directions;
            //directionsUnaAuno = this.directions.search('.');
            directionsUnaAuno = recipe.value.directions.split('.');
            //directionsUnaAuno = this.directions.split('\n');
            console.log("Directions Separados: "+directionsUnaAuno);
          /* let directionsUnaAuno = this.directions;
            directionsUnaAuno = this.directions.replace(/\s/g, ' .');
            console.log("Directions Separados: "+directionsUnaAuno);
          */
          //Indico si tiene el featured activado
            if(recipe.value.featured==''){
              recipe.value.featured = false;
            } else {
              recipe.value.featured = true;
            }
          //Creo el objeto receta para pasar a App 
              let receta = {
                  id: recipe.value.id, 
                  title: recipe.value.title, 
                  imageUrl: recipe.value.imageUrl, 
                  servings: recipe.value.servings,
                  time: recipe.value.time,
                  difficulty: recipe.value.difficulty,
                  ingredients: ingredienteUnaAuno,
                  directions: directionsUnaAuno,
                  featured: recipe.value.featured,
              };
          //Emite la receta nueva a App
             context.emit('nuevaReceta', receta);

          //Muestra datos de receta por consola
              console.log("Emitida nueva receta");
              console.log(receta);

          //Pongo el Div de error en blanco
              error.innerHTML = '';

          //Indico en el Div add que se añade la receta nueva
              add.innerHTML = 'Add recipe <br/>';
              add.innerHTML += '<br/>';
            borrarCampos();
          //Reinicio los campos
              // recipe = ref({
                // id: recipe.value.id='', 
                // title: recipe.value.title='', 
                // imageUrl: recipe.value.imageUrl='', 
                // servings: recipe.value.servings='',
                // time: recipe.value.time='',
                // difficulty: recipe.value.difficulty='',
                // ingredients: recipe.value.ingredients=[],
                // directions: recipe.value.directions=[],
                // featured: recipe.value.featured='',
              // });
          }//Fin if/else
    
        }//FIN createRecipe()
        const borrarCampos = () => {
          //Reinicio los campos
              recipe = ref({
                id: recipe.value.id='', 
                title: recipe.value.title='', 
                imageUrl: recipe.value.imageUrl='', 
                servings: recipe.value.servings='',
                time: recipe.value.time='',
                difficulty: recipe.value.difficulty='',
                ingredients: recipe.value.ingredients=[],
                directions: recipe.value.directions=[],
                featured: recipe.value.featured='',
              });
                }
      /* Aquest mètode s'ha d'executar quan es faci clic al botó que conté el svg amb
         el símbol X. S'encarregarà de:
          ○ Emetre un esdeveniment close-modal  */
        const closeForm = () => {
          context.emit('closeForm', showModal.value = false);
        }//FIN closeForm()

      return { createRecipe, closeForm, borrarCampos, recipe }
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

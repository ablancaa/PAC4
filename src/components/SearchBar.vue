<template>
  <div class="search">
    <!-- <p>Consulta: {{ consulta }}</p> -->
    <div>
    <form @submit.prevent="busqueda"> 
       <input type="text" id="consulta" v-model="consulta" @search="search" placeholder="Search for a recipe" />
       <button v-if="consulta.length" @click="clearSearch">Clear Search</button>
    </form>
    </div>
    <button @click="showForm">Add a new recipe</button>
    <div>
    </div>
  </div>
</template>
<script>

import { defineComponent, ref, watch} from "vue";
export default defineComponent({
  
  name: "SearchBar",
  components: {},
  emits: ['openForm', 'clearSearch', 'newVal'],
  setup(props, context){
    let showModal = ref(false);
    let consulta = ref('');
    console.log("Setup en SearchBar variable consulta: "+consulta.value)

    /* Aquest mètode s'encarregarà d'emetre un esdeveniment show-form. S’haurà
    d’executar quan es faci clic al botó “Add a new recipe”. */
    const showForm = () => {
      context.emit('openForm', showModal.value = true);
      console.log("Emitido de SearchBar: "+showModal.value);
    }
    
     /* Aquest mètode s'encarregarà de buidar l'element input del camp de cerca.
     S’haurà d’executar quan es faci clic al botó “Clear Search”. */
    const clearSearch = () => {
      consulta.value = document.getElementById("consulta").value="";
      //context.emit('clearSearch', consulta.value);
      console.log("Función clearSearch(){} Campo reseteado");
    }

     /*Aquest mètode s'executarà cada vegada que es modifiqui l'element
     input del camp de cerca (cada vegada que es teclegi una lletra). Emetrà un esdeveniment
     'search' amb el contingut del camp de cerca */
    const search = ref(consulta);   
    watch(search, (currentValue, oldValue) => {
      console.log(currentValue);
      console.log(oldValue);
      context.emit("newVal", search);
    });
  
    return { showForm, clearSearch, consulta, watch, search };

  },//FIN Setup()

});//FIN EXPORT DEFAULT
</script>

<style scoped>
.search {
  width: 100%;
  padding: 15px;
  display: flex;
  align-items: center;
  border-bottom: 1px solid #ccc;
  margin-bottom: 25px;
  justify-content: space-between;
}
.search input {
  width: 100%;
  width: 500px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 16px;
  margin: 0 auto;
  max-width: 500px;
}
.search button {
  margin-left: 10px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
  background: #4caf50;
  color: #fff;
}
</style>

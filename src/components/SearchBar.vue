<template>
  <div class="search">
    <!-- <p>Consulta: {{ consulta }}</p> -->
    <div>
    <form @submit.prevent="busqueda"> 
       <input type="text" id="consulta" v-model="consulta" @search="search" placeholder="Search for a recipe" />
       <button v-if="consulta != ''" @click="clearSearch">Clear Search</button>
    </form>
    </div>
    <button @click="showForm">Add a new recipe</button>
    <div>
    </div>
  </div>
</template>
<script>

import { defineComponent, ref } from "vue";
export default defineComponent({
  name: "SearchBar",
   components: {},
  /*data() {
    return {
      //consulta: '',

    }
  },*/
  setup(context){
    let showModal = ref(false);
    let consulta = ref('');
    console.log("Setup en SearchBar variable consulta: "+consulta.value)
    
    /* setTimeout(()=> {
         user.value='';
       }, 3000) */

    /* Aquest mètode s'encarregarà d'emetre un esdeveniment show-form. S’haurà
    d’executar quan es faci clic al botó “Add a new recipe”. */
    const showForm = () => {
      context.emit('openForm', showModal.value = true);
      console.log("Emitido de SearchBar: "+showModal.value);
    }
     /* Aquest mètode s'encarregarà de buidar l'element input del camp de cerca.
     S’haurà d’executar quan es faci clic al botó “Clear Search”. */
    const clearSearch = () => {
    consulta = document.getElementById("consulta").value="";
    context.emit('clearSearch', consulta.value)
    console.log("Función clearSearch(){} Campo reseteado");
    }
     /*Aquest mètode s'executarà cada vegada que es modifiqui l'element
     input del camp de cerca (cada vegada que es teclegi una lletra). Emetrà un esdeveniment
     'search' amb el contingut del camp de cerca */
    const search = (newVal)=>{
      console.log("Letra picada en Search Bar Input");
        if(newVal != ''){
          context.emit('newVal', consulta.value);
          console.log("Contenido de newVal: "+consulta.value);
        }
    }  
    
    return { consulta, showForm, clearSearch, search };
  },//FIN SETUP()
  computed: {},
  methods: {
    /* Aquest mètode s'encarregarà d'emetre un esdeveniment show-form. S’haurà
    d’executar quan es faci clic al botó “Add a new recipe”. */
    /*showForm(){
      this.$emit('openForm', this.showModal = true);
      console.log("Emitido de SearchBar: "+this.showModal);
    },*/
    /* Aquest mètode s'encarregarà de buidar l'element input del camp de cerca.
    S’haurà d’executar quan es faci clic al botó “Clear Search”. */
    /*clearSearch(){
      this.consulta = document.getElementById("consulta").value="";
      this.$emit('clearSearch', this.consulta);
      console.log("Función clearSearch(){} Campo reseteado");
    },*/
    /*Aquest mètode s'executarà cada vegada que es modifiqui l'element
    input del camp de cerca (cada vegada que es teclegi una lletra). Emetrà un esdeveniment
    'search' amb el contingut del camp de cerca */
    /* search(newVal){
      console.log("Letra picada en Search Bar Input");
      if(newVal != ''){
        this.$emit('newVal', this.consulta);
        console.log("Contenido de newVal: "+ this.consulta);
      }
    },*/
  }//FIN DE METHODS

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

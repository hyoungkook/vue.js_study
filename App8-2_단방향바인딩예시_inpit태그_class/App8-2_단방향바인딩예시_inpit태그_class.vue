<template>
  <div v-bind:class="nameClass">                   <!-- (1) class="name" -> v-bind:class="nameClass"으로 입력함 -->
    {{name}}           
  </div>
  <input v-bind:type="type" v-bind:value="name">    
                                                      

  <button class="btn btn-primary" v-on:click="updateName"> click </button>  
</template>

<script>
import {ref} from 'vue';                                                  

export default {
  setup() {
    const name = ref('Kossie');                                
    const type = ref('number');      
    const nameClass = ref('name');                /* (2) 입력함. nameClass라는 값이 name인데 return을 해주고, 
                                                         (1)의 nameClass에 name이 들어가게 된 것.              */

    const updateName = () => {
      name.value = 'Coder';                                              
      type.value = 'text';                  
    }

    return {                                                       
      name,
      updateName,  
      type, 
      nameClass,                                  // (3) 입력함                             
  };
}
}
</script>

<style>
 .name{
   color : blueviolet;
 }
</style>
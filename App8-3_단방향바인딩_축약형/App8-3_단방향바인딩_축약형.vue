<template>
  <div :class="nameClass">                           <!-- ":class = v-bind:class" (v-bind생략) -->
    {{name}}           
  </div>
  <input :type="type" :value="name">                 <!-- ":type = v-bind:type"  / ":value = v-bind:value" (v-bind생략)     -->
                                                      

  <button class="btn btn-primary" @click="updateName"> click </button>  
                                                     <!--"@click = v-on:click" ("v-on: = @") -->
</template>

<script>
import {ref} from 'vue';                                                  

export default {
  setup() {
    const name = ref('Kossie');                                
    const type = ref('number');      
    const nameClass = ref('name');              
                                             

    const updateName = () => {
      name.value = 'Coder';                                              
      type.value = 'text';                  
    }

    return {                                                       
      name,
      updateName,  
      type, 
      nameClass,                                                            
  };
}
}
</script>

<style>
 .name{
   color : blueviolet;
 }
</style>
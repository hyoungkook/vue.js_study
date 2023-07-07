<template>
  <div class="name">
    {{name}}                                                                  
  </div>
  <button class="btn btn-primary" v-on:click="updateName"> click </button>  
</template>

<script>
import {ref} from 'vue';                                                

export default {
  setup() {
    const name = ref({                    // (1) ref 입력함
      id : 1
    });                                   

    const updateName = () => {
      name.value.id = 2;                  // (2) ref를 쓸때는 '.value' 를 붙이고 id 접근해야
    }

    return {                          
      name,
      updateName,
  };
}
}
</script>

<style>
 .name{
   color : blueviolet;
 }
</style>

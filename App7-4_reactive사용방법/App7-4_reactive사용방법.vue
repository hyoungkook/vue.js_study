<template>
  <div class="name">
    {{name}}                                                                 
  </div>
  <button class="btn btn-primary" v-on:click="updateName"> click </button>     
</template>

<script>
import {reactive} from 'vue';    /* (1) ref 대신 reactive 입력함
                                        ref와 달리 reactive는 value를 쓰지 않는 방법이다. (편리함)
                                        object, array에 사용 가능(문자열, 숫자 사용 못함)   */  
                                          

export default {
  setup() {
    const name = reactive({       // (2) 변수를 ref대신 reactive로 감싸줬음 (여기서는 객체에 사용)
      id : 1
    });                                   

    const updateName = () => {
      name.id = 2;                // (3) ref와 달리 reactive 사용시에는 '.value' 를 붙일필요없이 바로 id 접근이 가능함
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

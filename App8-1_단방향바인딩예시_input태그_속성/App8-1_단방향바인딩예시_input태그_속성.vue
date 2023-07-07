<template>
  <div class="name">                 
    {{name}}           
  </div>
  <input v-bind:type="type" v-bind:value="name">  <!-- (1) input태그에 v-bind:속성 ="변수명" 를 통해 데이터바인딩을 해줌(원하는 변수를 사용가능)  
                                                       (4) 속성은 모두 바인딩이 되기 때문에, type="text" 도 마찬가지로 v-bind:type="type"로 바꿔줌 
                                                           template에서 태그의 속성들에 v-bind를 통해서 데이터 바인딩을 시킬 수 있다 -->

  <button class="btn btn-primary" v-on:click="updateName"> click </button>  
</template>

<script>
import {ref} from 'vue';                                                  

export default {
  setup() {
    const name = ref('Kossie');              // (2) input과 연결되는 변수를 입력함                   
    const type = ref('number');              // (5) input과 연결되는 변수를 입력함 (input이 number의 속성을 갖게됨)

    const updateName = () => {
      name.value = 'Coder';                  // (3) 함수가 실행되며 변수가 바뀌게 되는데, input도 같이 바뀜                              
      type.value = 'text';                   // (6) 함수가 실행되며 input의 속성이 number에서 text로 바뀜
    }

    return {                                                       
      name,
      updateName,  
      type,                                  // (6) 입력함
  };
}
}
</script>

<style>
 .name{
   color : blueviolet;
 }
</style>
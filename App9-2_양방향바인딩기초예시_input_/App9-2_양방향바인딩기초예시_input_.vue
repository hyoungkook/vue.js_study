<template>
  <input type="text" v-model="name" >  <!-- (1) 'v-model'을 통해 배경설명에서 나온 복잡한 과정 생략하고 양방향 바인딩가능(혁명적) -->
  <button class="btn btn-primary" @click="onSubmit"> click </button>  
  
</template>

<script>
import {ref} from 'vue';                                                  

export default {
  setup() {
    const name = ref('Kossie');          // (2) name값인 Kossie가 양방향으로 바뀜. 


    const onSubmit = () => {
      console.log(name.value)            // (양방향 바인딩이 일어나는지 콘솔에 찍어서 확인해보는 용도)
    };

    return {                                                       
      name,
      onSubmit,
  };
}
}
</script>

<style>
 .name{
   color : blueviolet;
 }
</style>
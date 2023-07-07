<template>

    <div v-show="toggle">true</div>                          <!--(4) 입력함, ref값이 true일때 true가 표시되고-->                     
    <div v-show="!toggle">false</div>                        <!--(4) 입력함, ref값이 false일때 false가 표시된다 -->        
    
    <button @click="onToggle">Toggle</button>                <!--(5) 입력함. 클릭시 함수가 실행                 -->

    </template>
    
    <script>
    import {ref } from 'vue'                                    // (1) ref 입력함
    
    export default {
        setup() {
            const toggle = ref(true);                          // (2) 입력함
    
            const onToggle = () => {                           // (6) 입력함                     
                toggle.value = !toggle.value;                  // (6) 입력함. 클릭시 함수가 실행되면서 ref값이 true와 false를 오가도록-->
            }
    
    
            return {
                toggle,                                        // (3) 입력함
                onToggle,                                      // (7) 입력함 
            }
    
        }};
  
    </script>
    
    <style>
    
    
    </style>
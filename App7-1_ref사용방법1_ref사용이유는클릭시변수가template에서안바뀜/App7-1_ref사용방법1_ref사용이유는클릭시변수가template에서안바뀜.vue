<template>
  <div class="name">
    {{name}}                                                                   
  </div>
  <button class="btn btn-primary" v-on:click="updateName"> click </button>  
                                   <!-- (5) 함수명에 아래에서 만든 updateName을 입력함 -->    

</template>

<script>
import {ref} from 'vue';                 

export default {
  setup() {
    let name = ref('Kossie Coder1'); /* (3) const라고 하면 변수가 바뀌지 않기 때문에 let으로 바꿈.  
                                             변수를 ref로 감싸줘야(ref안에는 문자열, 숫자, object, 배열이 들어가도 됨 ) */

/*
    const greeting = (name2) => {                        
      return 'hello,' + name2;
    };

    const greet = greeting(name);                       
*/

    const updateName = () => {   // (1) 클릭시 변수인 name값이 변경되도록 만들고 싶음. 함수를 만듬.
      name = 'Kossie Coder';     // (2) 처음에 name = 'Kossie Coder'; 라고 넣음
      console.log(name);         // (6) (5)번까지 하고나서 클릭을 했는데, 화면의 name값이 바뀌질 않아서, 
                              /* console.log(name);을 입력하고 개발자 도구를 보니 거기에서는 변수가 바뀌는 것을 확인. 
                                 근데 name값이 template에서는 적용이 안되고 있다.
                                 vue.js composition API를 사용할때 이렇게 일반변수를 사용하면 변수가 script쪽에서 바뀐다고 해도,
                                 template에서 바뀌지는 않는다. 그래서 vue.js에서는 ref라는 기능을 제공한다.  */
    };

    return {                                                     
      name,                    
      updateName,                // (4) 입력함
  };
}
}
</script>

<style>
 .name{
   color : blueviolet;
 }
</style>

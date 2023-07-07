<template>
  <div class="name">
    {{name}}                                                                   
  </div>
  <button class="btn btn-primary" v-on:click="updateName"> click </button>  
                                         <!-- 아래에서 만든 함수명 updateName을 입력함 -->
</template>

<script>
import {ref} from 'vue';                 // (1) 이 줄 전체를 입력함(vue패키지에서 ref라는 것을 불러오다)

export default {
  setup() {
    const name = ref('Kossie Coder1');   // (2) 값을 ref안에 넣어줌. (name이라는 변수선언은 const로 하네)
                                         /* ref안에는 위와 같이 string뿐만 아니라, ref(1) 처럼 숫자가 와도 되고,
                                            ref({}) 처럼 객체가 와도 되고, ref([]) 처럼 배열이 와도 된다. */
/*
    const greeting = (name2) => {                        
      return 'hello,' + name2;
    };

    const greet = greeting(name);                       
*/

    const updateName = () => {           // (3) 클릭시 발생하는 이벤트 함수 생성
      name.value = 'Kossie Coder';       /* (4) ref를 사용하면 바뀌는 변수 값에 .value를 입력해야 
                                                name값이 바뀌면서 template도 같이 업데이트 된다.
                                                template도 같이 변경되는 것을 reactive하다고 한다. 
                                                <template>안에서는 name.value처럼 적지 않고 사용한다. 
                                                */                     
    };

    return {                                                     
      name,                    
      updateName,                        // (5) 입력함
  };
}
}
</script>

<style>
 .name{
   color : blueviolet;
 }
</style>

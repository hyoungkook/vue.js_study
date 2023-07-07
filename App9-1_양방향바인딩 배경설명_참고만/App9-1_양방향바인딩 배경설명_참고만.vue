<template>
  <input type="text" :value="name" @input="updateName">  <!--(4) @input="updateName" 입력함 (input박스안에 값을 입력할때마다 함수가 실행(javascript의 oninput이벤트)) -->                     
  <button class="btn btn-primary" @click="onSubmit"> click </button>  
  
</template>

<script>
import {ref} from 'vue';                                                  

export default {
  setup() { 
    const name = ref('kossie');    /* (1) kossie라는 값을 바꾸게 되면 template을 통해 나타나는 화면도 같이 바뀐다. 
                                          그러나 input박스 값을 유저가 바꾸게 되면, 반대로 name의 값 kossie는 바뀌지 않는다. (단방향바인딩)
                                          여기있는 방법은 알 필요 없다. 단지 참고사항일뿐.                                                 */

    const onSubmit = () => {
      console.log(name.value)      // (6) 양방향으로 변경되는지 확인 (끝)
    };

    const updateName = (e) => {    // (2) 함수 만들었음 (함수를 실행할때 e(이벤트)라는 객체를 받아옴. e에는 input박스 자체에 대한 정보가 많이 들어있다)
      name.value = e.target.value; // (5) 양방향으로 값이 변경되도록 함.
    };


    return {                                                       
      name,
      onSubmit,
      updateName,                  // (3) 입력함
  };
}
}
</script>

<style>
 .name{
   color : blueviolet;
 }
</style>
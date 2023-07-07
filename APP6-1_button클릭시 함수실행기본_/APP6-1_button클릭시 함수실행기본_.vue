<template>
  <div class="name">
    {{name}}                                                                   
  </div>
  <button class="btn btn-primary" v-on:click="consoleLog"> click </button>     
  <!--(1)bootstrap을 넣은 것임. class="btn btn-primary"만 넣은것으로도 디자인 바뀌네. 물론 index.html에 링크 입력해야-->
  <!--(2)버튼 클릭이벤트를 만들고 싶다. button태그 속성에 v-on:click=""에서 ""부분에 함수를 넣으면 클릭시 함수가 실행됨 -->
  <!--(5)""안에 아래에서 만든 함수 이름을 넣음 -->

</template>

<script>
export default {
  setup() {
    const name = 'Kossie Coder1';    

/*
    const greeting = (name2) => {                        
      return 'hello,' + name2;
    };

    const greet = greeting(name);                       
*/

    const consoleLog = () => {            //(3) consoleLog라는 함수를 만들었음                     
      console.log('hello world');
    };

    return {                                                     
      name,
      consoleLog,                         //(4) 함수를 만들었으니 여기에도 입력해줘야지
  };
}
}
</script>

<style>
 .name{
   color : blueviolet;
 }
</style>
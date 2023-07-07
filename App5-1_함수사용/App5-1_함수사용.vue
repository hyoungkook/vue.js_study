<template>
  <div class="name">                                     
    {{greeting('hyoungkook')}}<br>       <!--(3) 함수이기 때문에 ()도 넣어야 함(매개변수는 넣든지 안넣든지) -->
    {{greet}}                            <!--(4) 아래에서처럼 함수를 변수로 선언하고 이렇게 사용해도 됨 -->   
  </div>
</template>

<script>
export default {
  setup() {
    const name = 'Kossie Coder1';

    const greeting = (name2) => {        // (1) greeting이라는 화살표 함수를 하나 만들었음. 매개변수도 넣음(안넣어도 됨)
      return 'hello,' + name2;
    };

    const greet = greeting(name);        // (4) greeting 함수를 이렇게 변수로 선언하고 사용해도 됨

    return {                                             
      name,
      greeting,                          // (2) 이렇게 return에 넣어줘야 greeting이라는 함수가 template에서 접근이 가능함.
      greet,                             // (5) 변수로 선언했으면 return에 넣어줘야 greet이라는 변수가 template에서 접근이 가능함.
    };
  }
}
</script>

<style>
 .name{
   color : blueviolet;
 }
</style>

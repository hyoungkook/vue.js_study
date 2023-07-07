<!--

지금까지 만든 To-do앱을 페이지로 이동하는 방법
(1) App.vue에 있는 소스를 전부 src/pages/todos/index.vue에 붙여넣는다.
(2) index.vue에서 import ~ from ~ 에 있는 주소에서 .을 @로 바꾼다(@는 src폴더를 가리킴)
(3) 기존의 App.vue는 아래와 같이 router만 남기고 다 지운다.
(4) 이제 src/router/index.js에 있는 path주소대로 페이지가 실행된다.

 -->

<template>
  <router-view/>
</template>

<script>                                                                                                 
export default {                                                         
  }              
</script>

<style>
 .todo{                                
   color : gray;
   text-decoration:line-through;
 }
</style>
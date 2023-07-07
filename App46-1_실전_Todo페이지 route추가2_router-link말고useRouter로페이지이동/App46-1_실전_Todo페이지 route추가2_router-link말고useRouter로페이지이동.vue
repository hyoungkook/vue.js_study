<!-- 
(1) vue.js에서 페이지간 이동을 할때는 아래와 같이 router-link 태그를 사용한다. 
    그런데 To-do카드를 클릭했을때 페이지를 이동하는 것은 router-link말고 다른 방법을 사용해보겠다.
    먼저 To-do list 컴포넌트 파일(TodoList6.vue)로 이동한다.
-->

<template>

<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <router-link class="navbar-brand" to="/">Daejoo-industry</router-link> 
                                                                                   
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>

  <div class="collapse navbar-collapse" id="navbarSupportedContent">
    <ul class="navbar-nav mr-auto">
      <li class="nav-item active">
        <router-link class="nav-link" to="/todos">Todos </router-link>      
                           
      </li>
    </ul>
  </div>
</nav>

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
<!-- 
       (1) router를 만들면서 src>router>index.js 파일안을 보면 라우터마다 name을 만들었는데,
           name을 이용해서도 페이지간 이동을 할 수 있다.
-->

<template>

<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <router-link class="navbar-brand" :to="{ name:'Home'}">Daejoo-industry</router-link> 
  <!-- (2) to="/" -> :to="{ name: 'Home'}" 입력함. 
           이렇게 해도 페이지간 이동 되는 것 바로 확인   
  -->
                                                                        
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>

  <div class="collapse navbar-collapse" id="navbarSupportedContent">
    <ul class="navbar-nav mr-auto">
      <li class="nav-item active">
        <router-link class="nav-link" :to="{name:'Todos'}">Todos </router-link>  
   <!-- (3) to="/todos" -> :to="{ name: 'Todos'}" 입력함. 
           이렇게 해도 페이지간 이동 되는 것 바로 확인   
           그럼 TodoList7.vue로 이동해서 name을 사용해서 페이지 이동하는 것을 해보겠다
  -->
                           
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
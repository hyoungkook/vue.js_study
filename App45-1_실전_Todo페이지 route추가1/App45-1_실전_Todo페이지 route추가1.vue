<!-- 
(1) Todo를 클릭하면 각각의 페이지가 나오도록 route로 만들어 보겠다. 
    src > router > index.js로 이동한다.

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
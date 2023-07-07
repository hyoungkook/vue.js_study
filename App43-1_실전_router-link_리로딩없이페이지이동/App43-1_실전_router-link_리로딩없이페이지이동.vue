
<template>

<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <router-link class="navbar-brand" to="/">Daejoo-industry</router-link> 
                                    <!-- (1) '#' -> '/' 입력          
                                         (2) a태그 대신 router-link태그 입력 
                                         (3) 'href -> 'to' 입력                    -->                                                           
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>

  <div class="collapse navbar-collapse" id="navbarSupportedContent">
    <ul class="navbar-nav mr-auto">
      <li class="nav-item active">
        <router-link class="nav-link" to="/todos">Todos </router-link>      
                                    <!-- (4) '#' -> '/todos' 입력     
                                         (5) a태그 대신 router-link태그 입력 
                                         (6) 'href -> 'to' 입력                    -->
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
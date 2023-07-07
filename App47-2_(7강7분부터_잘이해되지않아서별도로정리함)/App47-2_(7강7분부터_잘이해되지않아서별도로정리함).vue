
<!-- 47-2강 (1) 동영상 47강 7분부터 나오는 코드 변경은 잘 이해 되지 않아 별도로 정리함. 
               (todos/index.vue파일로 이동)                                                      -->
<template>

<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <router-link class="navbar-brand" :to="{ name:'Home'}">Daejoo-industry</router-link> 
                                                                        
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>

  <div class="collapse navbar-collapse" id="navbarSupportedContent">
    <ul class="navbar-nav mr-auto">
      <li class="nav-item active">
        <router-link class="nav-link" :to="{name:'Todos'}">Todos </router-link>  
                           
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
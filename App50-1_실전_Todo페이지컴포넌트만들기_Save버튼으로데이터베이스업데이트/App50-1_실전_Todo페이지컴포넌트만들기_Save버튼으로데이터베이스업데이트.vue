<template>

<!-- (1) save버튼을 눌렀을때, 데이터베이스에 업데이트를 해보도록 하겠다. 
         (50강)_id.vue로 이동                                                   -->
        
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
<div class="container">  
  <router-view/> 
</div>                
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
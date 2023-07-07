<template>
<!-- (1) 내용이 같을 경우, save버튼이 눌러지지 않도록 해보겠다. 
         (52강)_id.vue로 이동                                                            -->
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
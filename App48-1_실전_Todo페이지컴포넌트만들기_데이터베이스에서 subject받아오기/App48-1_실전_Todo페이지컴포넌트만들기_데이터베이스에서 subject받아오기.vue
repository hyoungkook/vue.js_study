
<!-- (1) Todo 페이지 템플릿을 만들어보겠다. -->


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
<div class="container">  <!-- (2) 입력함 (Todo Page글씨가 왼쪽 벽에 딱 붙어있는걸 떼어냄)        -->
  <router-view/> 
</div>                   <!-- (2) 입력함 (src>pages>todos>index.vue로 이동)                    -->
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
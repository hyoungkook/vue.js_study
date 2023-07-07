<!-- (1) 지난 영상에서 Todo카드를 누르면 Todo페이지로 이동하는 코드를 작성했는데, 
         delete버튼을 누르게 되면 Todo페이지로 이동하면서 삭제도 동시에 된다.
         이러한 현상이 일어나는 이유는 이벤트 버블링 때문이다.
         예를 들어 클릭이벤트를 걸어놓으면 클릭이벤트가 위로 전파가 된다. 클릭이벤트가 실행되고 나서
         부모태그로 가서 클릭이벤트가 있으면 실행시키고, 또 그 부모태그로 가서 클릭이벤트가 있으면 
         실행시키는 식으로 쭉쭉 올라가서 제일 위에있는 html 태그까지 실행시킨다. 
         (확인을 위해 TodoList8.vue파일로 이동)
-->

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
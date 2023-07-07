<template>

    <div class="col-md-9 ms-sm-auto col-lg-10 px-md-4">
      <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom">
        <h1 class="h2">공지게시판</h1>
      </div>
    
      <main class="container">
       
        <form class="d-flex" @submit.prevent="onSubmit">  <!-- 제출했을때 화면이 리로드됨(화면에 id뜨고 지저분. form의 특성). 
                                                               그래서 @submit뒤에 .prevent를 통해 리로드를 방지한다. -->
    
        <div class="flex-grow-1 mr-2">
          <input class="form-control" type="text" placeholder="Type new to-do" v-model="todo">
        </div>                             
    
        <div>
          <button class="btn btn-primary" type="submit"> Add </button>
        </div>                                
    
        </form>
        {{ todos }}
      </main>
    </div>
    
    </template>
    
    <script>
    import {ref} from 'vue';
    export default {
      setup(){
        const todo = ref('');   
        const todos = ref([]);   
        
        const onSubmit = () => {                 
          todos.value.push({
            id:Date.now(),
            subject:todo.value
          });                
        };
    
        return {
        todo,   
        todos,                                 
        onSubmit,                                
     
      }; 
    }
    }
    
    </script>
    
    <style>
    
    </style>
    
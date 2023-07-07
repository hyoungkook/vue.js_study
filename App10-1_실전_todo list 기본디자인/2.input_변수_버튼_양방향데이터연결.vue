<template>

    <div class="col-md-9 ms-sm-auto col-lg-10 px-md-4">
      <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom">
        <h1 class="h2">공지게시판</h1>
      
      </div>
    
      <main class="container">
       
        <form class="d-flex">
    
        <div class="flex-grow-1 mr-2">
          <input class="form-control" type="text" placeholder="Type new to-do" v-model="todo">
        </div>                                     <!--(2) v-model=변수"에서 변수이름을 todo로 넣음 -->
    
        <div>
          <button class="btn btn-primary" @click="onSubmit"> Add </button>
        </div>                                     <!--(3) @click="onSubmit" 입력함 -->
    
        </form>
    
      </main>
    </div>
    
    </template>
    
    <script>
    import {ref} from 'vue';
    export default {
      setup(){
        const todo = ref('');                       //(1) 변수이름을 todo로 잡고 empty string으로 넣음(input 박스에 아무것도 없도록)
        const onSubmit = () => {                    //(5) 입력함  
          console.log(todo.value)                   //(5) 입력함 (console찍어보면input입력되는 내용이 console에 뜬다. 양방향 데이터 확인!)
        };
    
        return {
        todo,                                       //(4) 입력함
        onSubmit,                                   //(4) 입력함
     
      }; 
    }
    }
    
    </script>
    
    <style>
    
    </style>
    
    
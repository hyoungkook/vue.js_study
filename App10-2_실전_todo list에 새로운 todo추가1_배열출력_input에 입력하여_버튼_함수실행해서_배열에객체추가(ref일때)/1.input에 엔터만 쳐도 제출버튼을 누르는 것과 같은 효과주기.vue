<template>

    <div class="col-md-9 ms-sm-auto col-lg-10 px-md-4">
      <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom">
        <h1 class="h2">공지게시판</h1>
      </div>
    
      <main class="container">
       
        <form class="d-flex" @submit="onSubmit">
                                       <!-- (2) 이 form 태그안에 button 태그에 있던 @click="onSubmit"을 옮겨왔음. 
                                                input박스에 입력후 엔터를 치든 버튼을 누르든 제출되는 효과.
                                                그런데 input을 클릭만해도 계속 제출이 되어서 empty array가 계속 뜬다.
                                                이것을 방지해 보자.  
                                            (4) @click="onSubmit" => @submit="onSubmit" 으로 변경한다.
                                        -->
    
        <div class="flex-grow-1 mr-2">
          <input class="form-control" type="text" placeholder="Type new to-do" v-model="todo">
        </div>                             
    
        <div>
          <button class="btn btn-primary" type="submit"> Add </button> 
                                        <!-- (1) 여기있던 @click="onSubmit"을 위의 form 태그로 옮긴다.
                                               form안의 submit버튼이 눌러졌을때 onsubmit 함수가 실행됨         
                                             (3) type="submit"추가함
                                            -->   
                                               
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
    
    
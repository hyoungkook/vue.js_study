<template>
    <div class="container">                          
      <h2>To-Do List</h2>                              
      <form @submit.prevent="onSubmit"> 
  
        <div class="d-flex">              <!-- (1) div태그를 만들어서 input과 button태그를 감싸줌       -->
                                          <!-- (2) 바로위 form태그에 있던 class="d-flex"를 옮겨옴(이렇게 해야 Error메시지가 버튼 밑으로 내려감) --> 
          <div class="flex-grow-1 mr-2">                    
          <input                                         
            class="form-control" 
            type="text" 
            v-model="todo"                       
            placeholder="Type new to-do">           
          </div>
  
          <div>                                           
            <button 
            class="btn btn-primary" 
            type="submit"> Add </button>            
          </div>
  
        </div>
  
        <div v-show="hasError" style="color:red"> This field cannot be empty</div>  
                                   
      </form>
                                              
      <div v-for="todo in todos" :key="todo.id" class="card mt-2">                       
        <div class="card-body p-2" >  
          {{ todo.subject }}               
        </div>
      </div>             
    </div>
  </template>
      
  <script>
  import {ref} from 'vue';                                                  
  
  export default {
    setup() {
      const hasError = ref(false);   
      const todo = ref('');                         
      const todos = ref([
        {id:1, subject:'휴대폰 사기'}, 
        {id:2, subject:'장보기'},     
      ]);            
  
      const onSubmit = () => {          
        if (todo.value === '') {     
          hasError.value = true;    
        } else {                   
          todos.value.push({             
          id:Date.now(),               
          subject:todo.value            
         });
          hasError.value = false;    
        }
       
      };
  
      return {                                                       
        todo,
        todos,                         
        onSubmit,
        hasError,                
    };
  }
  }
  </script>
  
  <style>
   .name{
     color : blueviolet;
   }
  </style>
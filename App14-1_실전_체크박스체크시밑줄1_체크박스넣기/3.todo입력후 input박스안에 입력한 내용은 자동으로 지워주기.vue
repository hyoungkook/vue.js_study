<template>
    <div class="container">                          
      <h2>To-Do List</h2>                              
      <form @submit.prevent="onSubmit" class="" > 
  
        <div class="d-flex">              
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
  
        <div class="form-check"> 
          <input 
            class="form-check-input" 
            type="checkbox"
            v-model="todo.completed"
            >        
          <label class="form-check-lavel">  
            {{ todo.subject }}    
          </label>        
        </div>
  
                         
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
      const todos = ref([]);            
  
      const onSubmit = () => {          
        if (todo.value === '') {     
          hasError.value = true;     
        } else {                  
          todos.value.push({             
          id:Date.now(),               
          subject:todo.value,
          completed:false,   
         });
          hasError.value = false;    
          todo.value="";        // 입력함. 추가버튼을 눌러서 todo를 추가하고 나면 input박스에 입력한 내용은 지워주는 기능
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
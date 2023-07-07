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
          <label class="form-check-lavel" :style="todoStyle">        
        <!-- (1) :style="todoStyle" 입력함                                              -->
                                   
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
      const todoStyle = {             // (2) style바인딩방법중에는 object를 이용하는 방법도 있고, array를 이용하는 방법도 있는데, 여기서는 object를 이용해서 하는 방법으로 한다.
                                      //     css에서는 text-decoration으로 해주는데, javascript에선 textDecoration.
        color:'gray'
      }
  
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
          todo.value="";        
        }
       
      };
  
      return {                                                       
        todo,
        todos,                         
        onSubmit,
        hasError,                    
        todoStyle,                      // (3) 입력함.     
    };
  }
  }
  </script>
  
  <style>
   .name{
     color : blueviolet;
   }
  </style>
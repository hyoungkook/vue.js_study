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
                                  <!-- (2) (1)에서 입력한 completed를 v-model을 통해 바인딩해줌  
                                           completed의 기본값이 false이기 때문에 todo를 추가했을때 체크가 안된 것으로 나타남.
                                           completed의 기본값을 true로 해주면 todo를 추가했을때 체크된 것으로 나타남. -->
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
          completed:false,      // (1) 처음 todo를 추가할때, 당연히 완료가 안되어 있어야 하므로 기본으로 false를 넣어줌.
                                //     (true로 해주면 체크박스가 체크된 상태로 나타남)
                                //     (id와 subject를 추가했는데, 이렇게 한가지를 더 추가함)
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
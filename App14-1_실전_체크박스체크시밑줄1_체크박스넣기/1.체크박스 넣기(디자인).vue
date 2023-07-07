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
  
        <div class="form-check">                           <!-- (1) 체크박스 넣기 시작. div태그 만들고, class="form-check" 넣기(부트스트랩) -->
          <input class="form-check-input" type="checkbox"> <!-- (2) input과 type 입력함 (클래스는 디자인). 체크박스는 생기는데 줄이 바뀌어서 나타남 -->     
          <label class="form-check-lavel">                 <!-- (3) 입력함 (클래스는 디자인) -->                      
            {{ todo.subject }}                             <!-- (4) 바로 아래 div태그안에 있던 걸 label안으로 옮겨줌. 줄이 바뀌는 문제 해결됨 -->
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
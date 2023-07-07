<template>
    <div class="container">                          
      <h2>To-Do List</h2>                      <!--  # 일단 시작은 TodoSimpleForm3.vue로 이동  -->        
      <TodoSimpleForm3 @add-todo="addTodo"/>   <!--  (5) @add-todo="함수이름" 입력함 -->
  
      <div v-if="!todos.length">                   
        추가된 todo가 없습니다 
      </div>
  
      <div v-for="(todo, index) in todos" :key="todo.id" class="card mt-2">   
              
        <div class="card-body p-2 d-flex align-items-center" >                            
          <div class="form-check flex-grow-1">  
            <input  
              class="form-check-input" 
              type="checkbox"
              v-model="todo.completed"
              >                   
            <label class="form-check-lavel" :class="{ todo:todo.completed }">            
              {{ todo.subject }}   
            </label>        
          </div>
            <div>               
              <button 
                class="btn btn-danger btn-sm"
                @click="deleteTodo(index)"
                >Delete
              </button>           
            </div>
          </div>
        </div>             
      </div>
  </template>
      
  <script>
  import {ref} from 'vue';           
  import TodoSimpleForm3 from './components/TodoSimpleForm3.vue';                                                                          
  
  export default {
    components: {                               
      TodoSimpleForm3                            
    },                                          
    setup() {
      const todos = ref([]);            
      const addTodo = (todo) => {           // (6) 입력함
                                            // (6) 자식컴포넌트에서 받아온 인자를(todo) 데이터로 넣어준다.
        todos.value.push(todo);             // (7) 입력함(자식컴포넌트로부터 전달된 todo를 todos array에 넣는다. 끝)
  
      };
  
  
      const deleteTodo = (index) => {  
        todos.value.splice(index, 1);  
      };
  
      return {                                                       
        todos,                         
        deleteTodo, 
        addTodo,                            // (6) 입력함                                                   
    };
  }
  }
  </script>
  
  <style>
   .todo{                                
     color : gray;
     text-decoration:line-through;
   }
  </style>
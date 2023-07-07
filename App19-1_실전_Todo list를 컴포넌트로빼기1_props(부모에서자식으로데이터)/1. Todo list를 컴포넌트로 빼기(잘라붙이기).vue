<template>
    <div class="container">                          
      <h2>To-Do List</h2>                              
      <TodoSimpleForm @add-todo="addTodo"/>   
      
      <div v-if="!todos.length">                   
        추가된 todo가 없습니다 
      </div>
                             <!-- (1) Todo list를 컴포넌트로 빼는 것을 해보겠다.
                                  (2) components폴더에 Todolist.vue 파일 만들기
                                  (3) 아래 부분을 잘라서 Todolist.vue에 붙여넣기(Todolist1.vue 파일로 gogo) -->
  <!--
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
      </div>                                                                                                       -->
  <TodoList :todos="todos" />                                   <!-- (7) <TodoList /> 입력함  -->
                         
    </div>
  </template>
      
  <script>
  import {ref} from 'vue';           
  import TodoSimpleForm from '../components/TodoSimpleForm.vue';          
  import TodoList from '../components/TodoList1.vue';              // (5) 입력함                                                               
  
  export default {
    components: {                               
      TodoSimpleForm,
      TodoList,                                                   // (6) 입력함                            
    },                                          
    setup() {
      const todos = ref([]);       
      const addTodo = (todo) => {         
        todos.value.push(todo);            
  
      };
  
  
      const deleteTodo = (index) => {  
        todos.value.splice(index, 1);  
      };
  
      return {                                                       
  
        todos,                         
        deleteTodo, 
        addTodo,                                                                           
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
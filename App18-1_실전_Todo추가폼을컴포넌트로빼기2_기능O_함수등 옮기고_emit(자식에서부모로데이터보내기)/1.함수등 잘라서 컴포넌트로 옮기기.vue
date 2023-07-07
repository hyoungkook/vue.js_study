<template>
    <div class="container">                          
      <h2>To-Do List</h2>                              
      <TodoSimpleForm2/>                       <!--  (8) 입력함 
  
      <form @submit.prevent="onSubmit" class="" >    (1) 왼쪽 폼태그 부분을 잘라내기 해서 TodoSimpleForm2.vue의 template에 붙여넣기
        <div class="d-flex">                             (TodoSimpleForm2.vue 로 이동)
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
      </form>                                                                                  -->
      
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
  import TodoSimpleForm2 from './components/TodoSimpleForm2.vue';                                                                          
  
  export default {
    components: {                               
      TodoSimpleForm2                            
    },                                          
    setup() {
      /* const hasError = ref(false);      // (3) hasError 여기에 있음
         const todo = ref('');             // (3) todo라는 데이터 여기에 있음
                                           // (3) 여기 있는 hasError와 todo를 잘라내서 TodoSimpleForm으로 옮기기 */
      const todos = ref([]);            
  
  /*
      const onSubmit = () => {             // (3) onSubmit 함수 여기에 있음
        if (todo.value === '') {           // (3) 여기 있는 onSubmit 함수를 잘라내서 TodoSimpleForm으로 옮기기
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
      };                                                 */
  
      const addTodo = (todo) => {        
                                           
        todos.value.push(todo);          
  
      };
  
  
      const deleteTodo = (index) => {  
        todos.value.splice(index, 1);  
      };
  
      return {                                                       
       // todo,                             // (7) 삭제함
        todos,                         
       // onSubmit,                         // (7) 삭제함
       // hasError,                         // (7) 삭제함
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
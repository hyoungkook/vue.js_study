<template>
  <div class="container">                          
    <h2>To-Do List</h2>                              
    <TodoSimpleForm @add-todo="addTodo"/>   
    
    <div v-if="!todos.length">                   
      추가된 todo가 없습니다 
    </div>
                                                                              
<TodoList :todos="todos" @toggle-todo="toggleTodo" @delete-todo="deleteTodo"/>  
  <!-- (6) 자식컴포넌트에서 보내온 index값을 받아보겠다.
           @delete-todo="deleteTodo" 입력함
           이벤트 이름 = "함수명"
           여기에 있는 함수명 deleteTodo는 (1)에 있는 deleteTodo이다. 끝.
-->
            
  </div>
</template>
    
<script>
import {ref} from 'vue';           
import TodoSimpleForm from './components/TodoSimpleForm.vue';          
import TodoList from './components/TodoList3.vue';                                                                      

export default {
  components: {                               
    TodoSimpleForm,
    TodoList,                                                                          
  },                                          
  setup() {
    const todos = ref([]);      
    const addTodo = (todo) => {         
      todos.value.push(todo);            

    };

    const toggleTodo =(index) => {   
      todos.value[index].completed = !todos.value[index].completed;                               
    }

    const deleteTodo = (index) => {  // (1) TodoList를 컴포넌트로 빼면서 삭제버튼이 동작을 안함. 동작하게 해보겠다. 
                                     //     deleteTodo 함수가 여기 부모컴포넌트에 있음. (TodoList3.vue로 이동)
      todos.value.splice(index, 1);  
    };

    return {                                                       

      todos,                         
      deleteTodo, 
      addTodo,                 
      toggleTodo,                                                                      
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


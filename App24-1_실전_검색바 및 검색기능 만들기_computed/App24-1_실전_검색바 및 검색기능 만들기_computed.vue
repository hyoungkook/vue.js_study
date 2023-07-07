<template>
  <div class="container">    

    <h2>To-Do List</h2>                 

    <input class="form-control" type="text" v-model="searchText" placeholder="Search">
                                        <!-- (1) 검색바를 만들어 보겠다. input 창 입력함 -->
                                        <!-- (4) v-model="todo" -> v-model="searchText" 로 변경 -->


    <hr />                              <!-- (5) 입력함 (디자인) -->
    <TodoSimpleForm @add-todo="addTodo"/>   
    
    <div v-if="!filteredTodos.length">  <!-- (12) !todos -> !filteredTodos로 변경.   끝.      -->          
      There is nothing to display
    </div>
                                                                              
<TodoList :todos="filteredTodos" @toggle-todo="toggleTodo" @delete-todo="deleteTodo"/>  

     <!-- (11) todos="todos" -> todos="filteredTodos" 로 변경. 
              기존의 TodoList에서 todos를 내려 보내줬는데, 이제 filteredTodos를 내려보내주도록 하겠다. -->
            
  </div>
</template>
    
<script>
import { ref, computed } from 'vue';  // (6) computed 입력함
import TodoSimpleForm from './components/TodoSimpleForm.vue';          
import TodoList from './components/TodoList4.vue';                                                                      

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

    const deleteTodo = (index) => {  
      todos.value.splice(index, 1);  
    };

    const searchText = ref('');              // (2) 입력함
    const filteredTodos = computed(() => {   // (7) 입력함
      if (searchText.value) {                // (8) 입력함. 먄약에 searchText.value가 있으면 (empty string이 아닐경우에)
                                             //     무엇을 return 해준다.
        return todos.value.filter(todo => {  // (9) 입력함. 여기 todo는 아무 이름이나 쓰면 됨
          return todo.subject.includes(searchText.value);
          // (10) todo안에 subject가 있다. todoSimpleForm.vue를 보면 (12)에 id, subject, completed 
          //      가 있다. todo가 object이기 때문에 object안에 subject가 있다. subject가 string인데, 
          //      string이 searchText.value를 포함할경우에 그때 filteredTodos안에 포함을 시키고, 
          //      searchText를 포함하지 않으면 제외를 시키는 것. 그럼 이 값을 template에서 사용해보겠다.
        });     
      }

      return todos.value;    // (9) 입력함. 만약에 searchText.value가 없으면(empty string일경우)는 
                             //     todos.value를 그대로 return 해준다.
    });

    return {                                                       

      todos,                         
      deleteTodo, 
      addTodo,                 
      toggleTodo,             
      searchText,           // (3) 입력함
      filteredTodos,        // (7) 입력함

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

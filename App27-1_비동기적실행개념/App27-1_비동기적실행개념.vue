<template>
  <div class="container">    

    <h2>To-Do List</h2>                 

    <input class="form-control" type="text" v-model="searchText" placeholder="Search">
   
    <hr />                        
    <TodoSimpleForm @add-todo="addTodo"/>   
    <div style="color:red">{{ error }}</div>    
    
    <div v-if="!filteredTodos.length">            
      There is nothing to display
    </div>
                                                                              
<TodoList :todos="filteredTodos" @toggle-todo="toggleTodo" @delete-todo="deleteTodo"/>  
            
  </div>
</template>
    
<script>
import { ref, computed } from 'vue';  
import TodoSimpleForm from './components/TodoSimpleForm.vue';          
import TodoList from './components/TodoList4.vue';          
import axios from 'axios';                                                                     

export default {
  components: {                               
    TodoSimpleForm,
    TodoList,                                                                          
  },                                          
  setup() {
    const todos = ref([]);      
    const error = ref('');       

    const addTodo = (todo) => {                     // (1) 비동기적 실행이란, 주문은 손님 1, 2, 3 순서로 했는데, 
                                                    //     음식은 손님 1, 3, 2 순서로 나오는 것. 
                                                    //    (손님2의 라면 물 끓이는데 시간이 걸리기 때문에)
                                                    //          

      error.value = '';                             // (2) 손님 1 (바로 실행됨)
      console.log('start')                          //     확인용으로 입력해놓음

      axios.post('http://localhost:3000/todos', {   // (3) 손님 2 (completed까지가 데이터베이스에 요청하는 부분. )
        subject: todo.subject,                      //     이걸 기다리는 동안 손님 3으로 넘어감
        completed: todo.completed,
                
       }).then(res => {             
           console.log(res);       
           todos.value.push(todo); 
                                   
       }).catch(err => {           
                                  
           console.log(err);        
           error.value = 'Something went wrong';   
       });

       console.log('hello')                       // (4) 손님 3 (확인용으로 입력해놓음)              
                                                  //     todo에 데이터를 입력하여 추가하면 console창으로 
                                                  //     손님 1, 3, 2 순으로 실행되는 것을 확인할 수 있다.       
    };        

    const toggleTodo =(index) => {   
      todos.value[index].completed = !todos.value[index].completed;                               
    }

    const deleteTodo = (index) => {  
      todos.value.splice(index, 1);  
    };

    const searchText = ref('');            
    const filteredTodos = computed(() => {  
      if (searchText.value) {              
        return todos.value.filter(todo => { 
          return todo.subject.includes(searchText.value);
 
        });     
      }

      return todos.value;  
    });

    return {                                                       

      todos,                         
      deleteTodo, 
      addTodo,                 
      toggleTodo,             
      searchText,           
      filteredTodos,        
      error,        

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

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
    const todos = ref([]);         // (1) todos앱을 사용하면 초기에 이와 같이 empty array를 사용하기 때문에,
                                   //     새로고침을 하면 데이터가 없다. db에 있는 데이터를 axios로 요청을 보내서 그 결과값을
                                   //     여기 todos array에 집어넣어 보겠다.     
    const error = ref('');       

    const getTodos = async () => { // (2) getTodos 함수 입력함. 그리고 axios요청을 보낼 것이기 때문에 async를 입력하고,

      try {                        // (4) 입력함. try, catch로 감싸줌.
        const res = await axios.get('http://localhost:3000/todos') 
          // (3) 입력함. await로 axios를 기다려 준다. 그리고 get request를 /todos로 보내준다. (모든 todos데이터를 달라는 뜻) 
          //     getTodos함수가 실행되면 비동기이기 때문에, 결과가 올때까지 await하고, 결과가 오면 변수 res에 담아주는 것    

        todos.value = res.data;    // (5) 입력함. 
                    
      } catch(err) {               // (4) 입력함. try, catch로 감싸줌.
        error.value = 'something went wrong';  
                                   // (6) 입력함. 데이터베이스가 죽었을때 메시지 표시.
      }                                              
    };

    getTodos();                    // (7) 입력함. (함수실행)

    const addTodo = async (todo) => {   
                                                    
      error.value = '';                           
      console.log('start')                        

      try {                         
        const res = await axios.post('http://localhost:3000/todos', {  
                                      
        subject: todo.subject,                     
        completed: todo.completed,
       });
       todos.value.push(res.data);        
      } catch(err) {                     
          console.log(err);        
          error.value = 'Something went wrong';   
      }
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
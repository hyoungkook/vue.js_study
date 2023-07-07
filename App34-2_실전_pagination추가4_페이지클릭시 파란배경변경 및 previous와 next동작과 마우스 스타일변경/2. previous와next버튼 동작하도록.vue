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
  
  <nav aria-label="Page navigation example">
    <ul class="pagination">              
      <li v-if="currentPage !==1" class="page-item">
          <a style="cursor:pointer" class="page-link" @click="getTodos(currentPage-1)">Previous</a></li>
                                <!-- (1) previous와 next버튼을 눌렀을때도 동작하게 해보겠다
                                         href="#" 대신에 @click="getTodos(currentPage-1)" 입력함(이제 작동하네)   --> 
                            
  
      <li v-for="page in numberOfPages" :key="page" 
           class="page-item" :class="currentPage === page ? 'active' : '' ">
          <a style="cursor:pointer" class="page-link" @click="getTodos(page)">{{page}}</a></li>
                        
    
      <li v-if="numberOfPages !== currentPage" class="page-item"><a style="cursor:pointer" class="page-link" 
          @click="getTodos(currentPage+1)">Next</a></li>
                                 <!--(2) href="#" 대신에 @click="getTodos(currentPage+1)" 입력함(이제 작동하네) 끝.-->
                                 
    </ul>
  </nav>        
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
      const numberOfTodos = ref(0);                      
      const limit = 5;                                   
      const currentPage = ref(1);         
                                                 
      const numberOfPages = computed(() => {       
        return Math.ceil(numberOfTodos.value/limit);
      });
                                                        
      const getTodos = async (page = currentPage.value) => {                    
        currentPage.value = page;                         
        try {                      
          const res = await axios.get(`http://localhost:3000/todos?_page=${page}&_limit=${limit}`); 
                                                             
          numberOfTodos.value = res.headers['x-total-count']; 
          todos.value = res.data;    
                      
        } catch(err) {               
          error.value = 'something went wrong';                          
        }                                              
      };
  
      getTodos();                   
  
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
  
      const toggleTodo = async (index) => {      
        error.value = '';                                  
        const id = todos.value[index].id;         
  
  
        try {                             
          await axios.patch('http://localhost:3000/todos/' + id, {  
                                           
            completed : !todos.value[index].completed
                                                                                              
          });       
           todos.value[index].completed = !todos.value[index].completed;        
                                              
        } catch (err) {                          
            console.log(err);                    
            error.value = 'Something went wrong';  
        }
        
      }
      const deleteTodo = async (index) => {                       
        error.value = '';                                     
        const id = todos.value[index].id;            
        
        try {                                                     
        await axios.delete('http://localhost:3000/todos/' + id);  
          
          todos.value.splice(index, 1);                        
        } catch (err){                                            
          console.log(err);                                      
          error.value = 'Something went wrong';                
        }
        
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
        numberOfPages,                               
        currentPage,     
        getTodos,                              
  
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
  
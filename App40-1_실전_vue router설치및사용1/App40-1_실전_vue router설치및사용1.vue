<template>
   <router-view/>            <!-- 입력함(vue router 사용가능)  
                                  src > router > index.js파일로 이동 -->
  <div class="container">    

    <h2>To-Do List</h2>                 

    <input 
      class="form-control" 
      type="text" 
      v-model="searchText" 
      placeholder="Search"
      @keyup.enter="searchTodo">  
    
    <hr />                        
    <TodoSimpleForm @add-todo="addTodo"/>   
    <div style="color:red">{{ error }}</div>    
    
    <div v-if="!todos.length">          
      There is nothing to display
    </div>
                             
<TodoList :todos="todos" @toggle-todo="toggleTodo" @delete-todo="deleteTodo"/>  
                                   
<nav aria-label="Page navigation example">
  <ul class="pagination">              
    <li v-if="currentPage !==1" class="page-item">
        <a style="cursor:pointer" class="page-link" @click="getTodos(currentPage-1)">Previous</a></li>                     

    <li v-for="page in numberOfPages" :key="page" 
         class="page-item" :class="currentPage === page ? 'active' : '' ">
        <a style="cursor:pointer" class="page-link" @click="getTodos(page)">{{page}}</a></li>
  
    <li v-if="numberOfPages !== currentPage" class="page-item"><a style="cursor:pointer" class="page-link" 
        @click="getTodos(currentPage+1)">Next</a></li>
                   
  </ul>
</nav>        
  </div> 
</template>

<script>                                     
import { ref, computed, watch } from 'vue'; 
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
    const searchText = ref('');  
    const numberOfPages = computed(() => {       
      return Math.ceil(numberOfTodos.value/limit);
    });

    const getTodos = async (page = currentPage.value) => {                    
      currentPage.value = page;                           
      try {                      
        const res = await axios.get(`http://localhost:3000/todos?_sort=id&_order=desc&subject_like=${searchText.value}&_page=${page}&_limit=${limit}`);                                         
                      
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
        await axios.post('http://localhost:3000/todos', { 
        subject: todo.subject,                     
        completed: todo.completed,
       });
       getTodos(1);                
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
        
        getTodos(1)                    

      } catch (err){                                            
        console.log(err);                                      
        error.value = 'Something went wrong';                
      }
      
    };

    let timeout = null     
    const searchTodo = () => {        
      clearTimeout(timeout);           
      getTodos(1);                     
      };                

    watch(searchText, () => {  
      clearTimeout(timeout);
      timeout = setTimeout(() => {   
        getTodos(1);        
      }, 2000)            
    });    

    return {                                                       
      todos,                         
      deleteTodo, 
      addTodo,                 
      toggleTodo,             
      searchText,              
      error,        
      numberOfPages,                               
      currentPage,     
      getTodos,   
      searchTodo,                                                             

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
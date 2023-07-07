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
import { ref, computed } from 'vue'; // (1) prop에서 확인해 보겠다. TodoList5.vue 파일로 이동
import TodoSimpleForm from './components/TodoSimpleForm.vue';          
import TodoList from './components/TodoList5.vue';          
import axios from 'axios';                                                                     

export default {
  components: {                               
    TodoSimpleForm,
    TodoList,                                                                          
  },                                          
  setup() {
    const todos = ref([]);      // (4) 처음에는 여기 배열안이 0이기 때문에 0이 찍히고, 
                                //     첫 페이지 todos 5개를 여기 배열에 담기 때문에, 5가 찍힌다.
                                //     그래서 todo list가 0개에서 5개로 변화하기 때문에 0과 5가 찍히는 것을 확인할 수 있다.
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
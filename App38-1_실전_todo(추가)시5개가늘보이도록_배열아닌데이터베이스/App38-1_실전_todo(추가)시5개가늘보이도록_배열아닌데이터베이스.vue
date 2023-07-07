<template>

  <div class="container">    

    <h2>To-Do List</h2>                 

    <input class="form-control" type="text" v-model="searchText" placeholder="Search">
   
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
        const res = await axios.get(`http://localhost:3000/todos?subject_like=${searchText.value}&_page=${page}&_limit=${limit}`); 
                      
        numberOfTodos.value = res.headers['x-total-count']; 
        todos.value = res.data;    
                    
      } catch(err) {               
        error.value = 'something went wrong';                          
      }                                              
    };

    getTodos();                   

        // (1) 새로운 todo추가시 todos배열에 추가하기 때문에, 페이지당 5개를 보여주기로 했음에도, 
        //     6개가 보이는 것을 확인할 수 있다. 

    const addTodo = async (todo) => {   
                                                    
      error.value = '';                           
      console.log('start')                        

      try {                         
        await axios.post('http://localhost:3000/todos', { // (4) "const res =" 삭제함
                                                          //     (2)를 삭제했기 때문에 필요없으므로
        subject: todo.subject,                     
        completed: todo.completed,
       });
       getTodos(1);                // (3) 입력함. getTodos(1) 함수를 한번더 실행함.
 //    todos.value.push(res.data); // (2) 삭제함(이렇게 push를 통해 배열에 추가했음을 확인할 수 있다)
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
                                                  
    watch(searchText, () => {  
      getTodos(1);             
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

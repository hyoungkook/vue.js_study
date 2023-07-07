<template>

  <div class="container">    

    <h2>To-Do List</h2>                 

    <input class="form-control" type="text" v-model="searchText" placeholder="Search">
   
    <hr />                        
    <TodoSimpleForm @add-todo="addTodo"/>   
    <div style="color:red">{{ error }}</div>    
    
    <div v-if="!todos.length">          <!-- (3) filteredTodos -> todos로 변경입력                 -->     
      There is nothing to display
    </div>
                             
<TodoList :todos="todos" @toggle-todo="toggleTodo" @delete-todo="deleteTodo"/>  
                                        <!-- (2) filteredTodos -> todos로 변경입력                 -->

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
import { ref, computed, watch } from 'vue';  // (4) watch 입력함
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
                                 // (1) 아래코드가 검색기능(search바)인데 맨처음 페이지에 있는 5개 내에서만
                                 //     검색을 하고 있어서 무용지물. 다른 페이지에 있는 것은 검색이 안됨.
                                 //     그래서 다른 페이지에 있는 것도 검색이 되게하기 위해서, 
                                 //     검색시 데이터베이스에 요청을 보낼때, 검색한 텍스트도 같이 요청을 
                                 //     보낸다. 그래서 보낸 텍스트를 포함한 todos를 필터해서 받을 수 있도록 해보겠다.
    const searchText = ref('');                                
    
    watch(searchText, (current, prev) => {    // (5) 입력함 (current는 현재값, prev는 이전값)
      console.log(current, prev);             // (6) 입력함
                                              //     여기까지 하고, search에 어떤 텍스트를 입력하면, 
                                              //     searchText = ref('')가 업데이트가 되면서, 
                                              //     watch가 searchText를 보고있다가 변화가 감지되면, watch뒷부분이
                                              //     실행되면서 console에 찍히게 된다.
    });                                       //     이제 searchText가 바뀔때 데이터베이스에 요청을 해서 필터가 된
                                              //     todos를 받아와 보도록 하겠다. 

    
    /*           
    const filteredTodos = computed(() => {  
      if (searchText.value) {              
        return todos.value.filter(todo => { 
          return todo.subject.includes(searchText.value);
 
        });     
      }
      return todos.value;  
    });                                       */

    return {                                                       
      todos,                         
      deleteTodo, 
      addTodo,                 
      toggleTodo,             
      searchText,           
      // filteredTodos,        
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

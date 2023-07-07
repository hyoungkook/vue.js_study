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
               <!--(1) 페이지를 클릭했을때 해당 페이지의 정보를 가져오게 하겠다. -->                
<TodoList :todos="filteredTodos" @toggle-todo="toggleTodo" @delete-todo="deleteTodo"/>  

<nav aria-label="Page navigation example">
  <ul class="pagination">
    <li v-if="currentPage !==1" class="page-item"><a class="page-link" href="#">Previous</a></li>


    <li v-for="page in numberOfPages" :key="page" 
         class="page-item" :class="currentPage === page ? 'active' : '' ">
        <a class="page-link" @click="getTodos(page)">{{page}}</a></li>
              <!-- (2) href="#"을 삭제하고, 클릭이벤트 넣기 @click="getTodos()"      
                   (6) getTodos() 안에 page를 입력함                                
                       getTodos(page)에 있는 page는 현재 페이지. 요 문단을 v-for로 돌리고 있다
                       페이지숫자(1,2,3)를 클릭할때마다 getTodos()함수가 실행되는데 이때 페이지 값을 함수에 넘겨준다
                       그럼 값을 가지고 데이터베이스에 요청을 해서 그 페이지에 맞는 데이터를 가져오게 된다. 
                   (7) 이제 페이지를 누르면 거기에 맞는 데이터를 가져온다. 근데 눌러도 파란 배경이 안 바뀜.  끝. -->


    <li v-if="numberOfPages !== currentPage" class="page-item"><a class="page-link" href="#">Next</a></li>
    
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
                                                      
    const getTodos = async (page = currentPage.value) => {  // (3) page라는 매개변수를 입력하고 디폴드 값으로 currentPage.value를 입력함
                                                            //     (page = currentPage.value 입력함)

      try {                      
        const res = await axios.get(`http://localhost:3000/todos?_page=${page}&_limit=${limit}`); 
                                                            // (4) 기존 currentPage.value자리에 page를 대신 입력함.
                                                            //     이제 어떤 인자(currentPage)를 받아오느냐에 따라서 가져오는 데이터(page)가 달라진다.
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
      getTodos,           // (5) 입력함                              

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

   
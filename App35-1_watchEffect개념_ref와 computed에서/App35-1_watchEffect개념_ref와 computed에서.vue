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
import { ref, computed, watchEffect } from 'vue'; // (1) watchEffect에 대해 알아보자. watchEffect를 사용하려면 
                                                  //     vue package에서 watchEffect를 import해서 setup function
                                                  //     아래에서 사용하면 됨.
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
/*
    watchEffect(() => {               //   (1) 입력함
      console.log(currentPage.value); //   (2) 원하는 로직 입력. 여기서는 현재 페이지를 console에 찍어주는 로직.
      console.log(numberOfTodos.value); // (3) 입력함. watchEffect안에 2개의 로직이 생겼는데, 이중 1개만 바뀌어도 둘다 실행됨.
    });                      */           

    //   (3) (2)에 대한 관련 설명임.
    //       currentPage가 ref를 사용하고 있기 때문에 .value를 해줘야 한다. 
    //       여기서 개발자 도구 console에 1이 찍히는 것을 볼 수 있음. 바로위 currentPage에서 1이라고 해놓았기 때문이지. 
    //       그런데 만약 watchEffect함수안에 reactive state가 있고, 그 값이 변경되면 watchEffect 내부가 다시 실행이 된다.
    //       여기서는 currentPage의 값이 바뀔때마다 console에 찍히게 된다.
    //       (페이지를 변경해서 클릭하면 watchEffect가 감지하고 console에 변경된 페이지가 찍힘)

    const numberOfPages = computed(() => {       
      return Math.ceil(numberOfTodos.value/limit);
    });
                                                      
    watchEffect(() =>  {              // (4) 입력함. 이번엔 computed의 값이 변경될때 watchEffect 내부가 실행 되는 것을 확인해 보겠다.
      console.log(numberOfPages.value);//(5) 입력함. numberOfPage는 처음엔 0인데, Todos를 불러오면서 계산하면 페이지수로 바뀐다. 
  });                                 //     그래서 console찍어보면 0이먼저 찍히고 numberOfPages가 바뀌면서 페이지수가 또 한번 찍힌다.
                                      // (6) 하는김에 reactive랑 pros도 변화를 watchEffect에서 감지하는 것을 확인해 보겠다.
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

   
   

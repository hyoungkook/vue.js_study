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
import { ref, computed, watch } from 'vue'; // (2) 입력함(watch)
import TodoSimpleForm from './components/TodoSimpleForm.vue';          
import TodoList from './components/TodoList4.vue';          
import axios from 'axios';                                                                     

  //(1) watch도 watchEffect와 비슷하게 reactive state를 지켜보다가
  //    업데이트가 되면 변화를 감지하고 어떤 로직을 실행시켜 주는 기능이다.
  //    홈피가면 한개의 소스를 지켜보는 방법과 여러개의 소스를 지켜보는 방법 2가지가 나와있다(노트참고)
  //    한개의 소스를 지켜보는 방법에 reactive를 사용하는 방법과 ref를 사용하는 방법이 있는데
  //    ref를 사용하는 방법을 먼저 해보겠다.(아래는 공식)
  //     
  //    const count = ref(0)
  //    watch(count, (count, preCount) => {
  //    /*...*/
  //    }) 
  //   
  //    watch의 첫번째 인자로 ref의 변수값(count)을 넣어주고, 그리고 함수를 넣으면 된다. 
  //    함수의 첫번째는 변경된 값(count)이 들어오고, 두번째는 변경되기 전의 값을 가져온다.(prevCount)

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
    watch(currentPage, () => {                  //(3) 입력함. currentPage만 변경되었을때 이 함수가 실행됨.
      console.log('hello')                      //    currentPage가 변경될때마다 hello라는 console이 찍힘.
    });
                                                                                                   */
    watch(currentPage, (currentPage, prev) => { //(4) 입력함 (괄호안은 변경된 값, 변경되기 전의 값)
      console.log(currentPage, prev)            //(5) 입력함 (1페이지에서 2페이지로 가면 console에 2 1이 찍힘)
    });
    

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

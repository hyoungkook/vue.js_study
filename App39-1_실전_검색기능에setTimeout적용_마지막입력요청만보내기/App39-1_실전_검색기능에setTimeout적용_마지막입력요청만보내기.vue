<template>

  <div class="container">    

    <h2>To-Do List</h2>                 

    <input 
      class="form-control" 
      type="text" 
      v-model="searchText" 
      placeholder="Search"
      >     
    
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
     
    // (1) 예를 들어 검색창에 "new" 라고 입력하면, n과 e와 w를 한글자를 입력할때마다 계속 요청을 보낸다.
    //     new를 다 입력했을때 한번에 요청을 보내면 낭비가 없다. 그래서 이것을 위해 javascript의
    //     setTimeout을 사용해서 n을 입력하면 예를 들어 2초후에 요청을 보내고, e를 입력하면 그전의
    //     n을 입력했던 요청을 사라지고 다시 2초후에 요청을 보내면 낭비가 없을 것이다. 

    let timeout = null      //(3) 입력함           
    watch(searchText, () => {  
      clearTimeout(timeout);//(3) 입력함
                            //    처음에 searchText가 변경이 되면, 
                            //    일단 시작하자마자 clearTimeout을 한다. 
                            //    그래서 기존에 timeout이벤트 걸려있는게 있으면 취소를 해준다. 
      timeout = setTimeout(() => {    
                            //(2) "setTimeout(() => { " 입력함
                            //(3) "timeout =" 입력함

        getTodos(1);        //(2) 기존에 watch에 들어있던 것을 setTimeout안에 넣어준다.
      }, 2000)              //(2) 입력함 (2000은 2초를 뜻함)
                            //    setTimeout은 안에 함수가 들어가고, 두번째는 시간이 들어간다.
                            //    2초가 흐른후에 안에 있는 함수(getTodos(1)가 실행된다
                            //    근데 여기까지한후 new를 입력하면 2초를 기다리기는 하는데, 
                            //    요청을 세번보낸다. new를 입력했을때, 앞에 입력한 n과 e는 취소해야 함.
    });    
    //(4) 다시 정리하자면, 검색창에 입력하면 searchText가 바뀌었기 때문에 watch내부가 실행이 되는데,
    //    clearTimeout을 한다(처음에는 null이기 때문에 아무것도 하지 않는다.)
    //    그리고 나서 setTimeout을 해서 2초후에 getTodos(1)를 실행하게 되는데, 2초가 되기전에
    //    검색창에 다시 입력을 하면 searchText가 바뀌어서 watch내부가 다시 실행되게 되고, 
    //    clearTimeout(timeout)이 실행되며 기존의 2초 timeout을 clear해버리며 기존꺼는 취소된다.
    //    근데 마지막에서도 2초를 기다려야 하니까, 기다릴필요없이 엔터를 누르면 요청을 보내도록 해보겠다.
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

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
    <li v-if="currentPage !==1" class="page-item"><a class="page-link" href="#">Previous</a></li>
    <!--                     (10) v-if="currentPage !==1" 입력함
                                  1페이지가 아닐때만 previous가 나타나도록    -->

    <li v-for="page in numberOfPages" :key="page" class="page-item" :class="currentPage === page ? 'active' : '' "><a class="page-link" href="#">{{page}}</a></li>
    <!--                      (6) v-for="page in 2" :key="page" 를 입력함(page를 1부터 2까지 돌리라는 얘기) 
                                  기존에 입력되어 있던 내용은 <li class="page-item"><a class="page-link" href="#">1</a></li>
                                  
                              (7) 숫자 1이 있던 곳에는 {{page}}를 입력함 
                              (8) page in 2 -> page in numberOfPages로 입력 
                              (9) :class="currentPage === page ? 'active' : '' " 입력함
                                  만약 currentPage가 page와 같으면 active(파란배경효과)를 넣어주고, 아니면 아무것도 안넣어준다 -->

<!--                          (5) 반복되는 것은 지워줌
    <li class="page-item"><a class="page-link" href="#">2</a></li>
    <li class="page-item"><a class="page-link" href="#">3</a></li>
-->

    <li v-if="numberOfPages !== currentPage" class="page-item"><a class="page-link" href="#">Next</a></li>
    <!--                     (11) v-if="numberOfPages !== currentPage" 입력함
                                  마지막페이지가 currentPage가 아닐경우에만 next가 나타나도록.  끝.  -->
  </ul>
</nav>        
  {{numberOfPages}}  <!--    (4)입력함. 이건 시범적으로 입력해 놓은 것임. 의미 없음.             -->
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
    const currentPage = ref(3);                   // (4-1) : 변수명만 page -> currentPage로 변경함(이게 더 말이 된다고)
    
                                                  //(1) (지난 강의에 이어서) 데이터베이스에 있는 데이터 숫자에 따라 
                                                  //    페이지 갯수가 바뀌도록 다이나믹하게 만들어보겠다.
    const numberOfPages = computed(() => {        //(2) 입력함
      return Math.ceil(numberOfTodos.value/limit);//    numberOfTodos를 limit로 나누면 page갯수가 나온다.   
                                                  //    올림을 해야 하기 때문에 Math에서 ceil()메소드가 사용됨.
                                                  //    여기서는 computed를 사용했기 때문에,computed함수안에 있는
                                                  //    reactive state(numberOfTodos)가 바뀔때마다 return ~ 줄이 다시 
                                                  //    실행이 되어서 numberOfPages가 업데이트 된다. 
    });
                                                      
    const getTodos = async () => { 

      try {                      
        const res = await axios.get(`http://localhost:3000/todos?_page=${currentPage.value}&_limit=${limit}`);  // (4-2) 변수명만 page -> currentPage로 변경함(이게 더 말이 된다고)  

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
      numberOfPages,                                 // (3) 입력함
      currentPage,                                   // (9) 입력함

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


   
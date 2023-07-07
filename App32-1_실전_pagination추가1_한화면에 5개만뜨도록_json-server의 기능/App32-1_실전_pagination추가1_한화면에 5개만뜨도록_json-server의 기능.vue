<template>

<!--
(1) json-server npm페이지에 들어가게 되면, 여러가지 기능중에서 pagination기능이 있다.(노트 참고)
    기본적으로는 10개의 아이템을 보내주는데, limit를 설정하면 예를들어 한쪽수에 20개씩 보내준다.
    "GET /posts?_page=7"
    "Get /posts?_page=75_limit=20""                                                
    한 페이지당 5개씩 받아오는 것으로 요청을 만들어 보겠다.                                 -->

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

                                    <!-- (3) 부트스트랩에서 pagination 스타일을 빌려와서 사용해 보겠다        
                                         (4) bootstrap pagination이라고 검색해서 나오는 것(노트참고)을 보고
                                             그대로 붙여넣기(아래와 같이)하면, 화면 하단에 페이지 넘어가는 단추가 뜬다. 
                                             아래 붙여넣기 한 코드는 페이지가 무조건 3개만 뜨는 하드코딩이 되어 있는데, 
                                             내가 넣은 데이터에 따라서 동적으로 바뀌는 페이지로 만들어야 한다.            -->
<nav aria-label="Page navigation example">
  <ul class="pagination">
    <li class="page-item"><a class="page-link" href="#">Previous</a></li>
    <li class="page-item"><a class="page-link" href="#">1</a></li>
    <li class="page-item"><a class="page-link" href="#">2</a></li>
    <li class="page-item"><a class="page-link" href="#">3</a></li>
    <li class="page-item"><a class="page-link" href="#">Next</a></li>
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
    const numberOfTodos = ref(0);                       // (5) 입력함
    const limit = 5;                                    // (6) 입력함
    const page = ref(1);                                // (7) 입력함(페이지는 초기값이 1)
                                                        //     이 페이지를 (8)에 넣어준다.
    
    const getTodos = async () => { 

      try {                      
        const res = await axios.get(`http://localhost:3000/todos?_page=${page.value}&_limit=${limit}`); 
      // (2) 'http://localhost:3000/todos' -> 'http://localhost:3000/todos?_page=1&_limit=5' 입력함
      //     (첫번째 페이지를 받아오고, 한 페이지당 5개씩 받아오겠다) => 그렇게 나타남
      //     그러나 두번째 페이지로 갈 수가 없기 때문에, 2페이지로 가는 버튼이 있어야지. 그게 pagination.

      // (8) 'http://localhost:3000/todos?_page=1&_limit=5' -> `http://localhost:3000/todos?_page=1&_limit=5`
      //     작은 따옴표를 백틱으로 바꿔준다. 그러면 안에 javascript변수를 넣어서 연결할 수 있다.
      // (9) `http://localhost:3000/todos?_page=1&_limit=5` -> `http://localhost:3000/todos?_page=${page.value}&_limit=${limit}`
      // (10) 여기까지 하면 한 화면에 5개만 뜨기는 하는데, 아래 페이지 버튼을 눌러도 안넘어감.  끝.

        numberOfTodos.value = res.headers['x-total-count']; // (6) 입력함
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

   

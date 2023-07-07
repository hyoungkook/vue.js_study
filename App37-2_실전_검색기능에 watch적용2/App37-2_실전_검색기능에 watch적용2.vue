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
    const searchText = ref('');  //(3) 맨아래에서(주석처리함) 잘라서 여기에 붙여넣었음    
                                 //    (이게 아래쪽에 있으니까 브라우저에 todos가 안뜨네)
    const numberOfPages = computed(() => {       
      return Math.ceil(numberOfTodos.value/limit);
    });

    const getTodos = async (page = currentPage.value) => {                    
      currentPage.value = page;                           
      try {                      
        const res = await axios.get(`http://localhost:3000/todos?subject_like=${searchText.value}&_page=${page}&_limit=${limit}`); 
                            /* (1) json-server의 npm 설명서를 보면 아래 공식이 있다(노트참고)
                                   "GET /post?title_like=server"
                                   우리는 db의 subject를 필터할 것이기 때문에, 위 공식에서 
                                   /post?subject_like=원하는 searchText를 하면 됨.
                               (2) 공식을 써서 subject_like=${searchText.value}& 를 위에 추가 입력함
                              */
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
                          
    // const searchText = ref('');                               
    
    watch(searchText, () => {  // (4) 입력함.
      getTodos(1);             // (4) 입력함. 인자로는 1을 넣어서 검색할때마다 첫페이지를 보여줄 수 있게 함.
    });    

      /* (5) 지금까지 내용을 총정리하면, 처음 앱이 시작하면 getTodos()를 실행해서 todos를 가져오는데, 
      처음에 searchText가 const searchText=ref('')에서와 같이 empty string이다. 그래서 (1)에서 empty string를
      보내면 모든 todos를 가져오게 된다. 
      그후 만약 3을 검색하게 되면, const searchText=ref('')가 empty string에서 3으로 바뀌게 되고, 
      watch는 searchText를 보고 있다가 변경을 감지하고 getTodos(1)이 실행된다. 
      그래서 (1)에서 ${searchText.value}값이 3이 들어가고 page는 1이 들어간다. limit은 항상 5로 설정했기 때문에 
      5가 들어감. 이렇게 요청을 보내면 db에서 subject가 3인것만 추려서 응답을 한다. (8분 9초부터 정리멘트)       */
             
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

<template>
  <div class="container">    

    <h2>To-Do List</h2>                 

    <input class="form-control" type="text" v-model="searchText" placeholder="Search">
   
    <hr />                        
    <TodoSimpleForm @add-todo="addTodo"/>   
    
    <div v-if="!filteredTodos.length">            
      There is nothing to display
    </div>
                                                                              
<TodoList :todos="filteredTodos" @toggle-todo="toggleTodo" @delete-todo="deleteTodo"/>  
            
  </div>
</template>
    
<script>
import { ref, computed } from 'vue';  
import TodoSimpleForm from './components/TodoSimpleForm.vue';          
import TodoList from './components/TodoList4.vue';          
import axios from 'axios';            // (4) 입력함 (axios 설치후)                                                            

export default {
  components: {                               
    TodoSimpleForm,
    TodoList,                                                                          
  },                                          
  setup() {
    const todos = ref([]);      

    const addTodo = (todo) => {       // (1) 현재는 todo를 추가할때 addTodo 함수가 실행되면서, todos array에 todo를
                                      //     추가만 해주고 있는데, 그 전에 먼저 데이터베이스에 todo를 저장해주고 todos array에
                                      //     추가해 보는 것을 해보겠다.
                                      // (2) 데이터베이스에 저장을 할 때는 post request를 통해서, 데이터 베이스에 
                                      //     데이터를 저장해줘 그러면 응답으로 데이터베이스에 저장을 완료했어 라고 오면, 그다음에
                                      //     todos array에 todo를 추가하겠다. 
                                      // (3) post request를 보낼때는 axios라는 npm 패키지를 설치해 사용
                                           
      axios.post('http://localhost:3000/todos', {  
        subject: todo.subject,
        completed: todo.completed,
               // (6) 입력함. post 괄호안에는 첫째로 보낼 url을 입력함(복사해서 붙여넣음)
               //     그리고 route가 post이기 때문에 /todos를 입력함 (홈피에 나옴. 노트참고)
               // (7) 괄호의 두번째에는 보내고자 하는 데이터가 들어감. todo는 addTodo함수와 연결된 TodoSimpleForm.vue에 가보면 
               //     id, subject, completed로 이루어져 있음.  json서버를 사용할때는 id를 보내지 않아도 id를 저절로 추가함.
               //     (MYSQL등도 그렇다고 함) (여기까지 하고 나서 db를 실행 시키면 db로 데이터가 들어간다)
              //  (8) src폴더에 db.json파일을 만들고 todos empty array를 미리 입력해 놓아야 함.  끝.
         
       });
        todos.value.push(todo);    // (5) (6)번 자리에 있던 것인데 여기로 옮겨줌((6),(7)에서 데이터베이스에 todo를 저장해주고, 여기서 todos array에 추가함)                                                                                
    };

    const toggleTodo =(index) => {   
      todos.value[index].completed = !todos.value[index].completed;                               
    }

    const deleteTodo = (index) => {  
      todos.value.splice(index, 1);  
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

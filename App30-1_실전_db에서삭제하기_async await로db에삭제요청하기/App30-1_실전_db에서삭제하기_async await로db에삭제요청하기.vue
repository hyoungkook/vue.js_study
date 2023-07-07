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
                /* (1) db에서 데이터를 삭제해 보겠다. 지금 delete버튼 누르면 화면상에서는 삭제가 되는데, db에서는 삭제가 안된다. 
                       지금은 다만 ref[] 에서만 삭제해 주고 있는 것이라서 새로고침을 하면 다시 화면에 나타난다.     
                       그래서 db에서 삭제하면 ref[] 에서도 삭제해 보겠다.                                                */
    
    const getTodos = async () => { 

      try {                      
        const res = await axios.get('http://localhost:3000/todos') 
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


    const toggleTodo =(index) => {   
      todos.value[index].completed = !todos.value[index].completed;                               
    }

    const deleteTodo = async (index) => {                       //(8) async 입력함
        error.value = '';                                       //(12) 입력함. 'Something went wrong라는 에러메시지가 사라지지 않아서. 끝.                                
        // (2) deleteTodo 함수를 보면 배열에서만 삭제하고 있음. db에 삭제요청을 보내서 db에서 삭제 확인후 배열에서 삭제해 보겠다.
        // (3) 홈페이지 json server를 보면 Delete를 하고 싶을땐 /todos/1(지우고 싶은 id) 처럼 적어주면 된다. (노트참조)

      const id = todos.value[index].id;            
        //(5) 입력함. todos value에서 index를 통해서 todo를 찾아낸다음 그것의 id를 가져와서 
      
      try {                                                     //(9) try로 감싸줌
      await axios.delete('http://localhost:3000/todos/' + id);  //(8) await 입력함
        //(4) 입력함. todos/ 다음에 id를 적어야 하는데, deleteTodo함수를 보면 index를 가지고 있다.
        //(6) (맨뒤) 여기에 넣어주겠다.
        //(7) axios delete요청을 이 주소로 보내게 되는데, id를 지워달라는 요청을 보내게 된다.
        todos.value.splice(index, 1);                           // (11) catch 다음단락에 있던 배열에서 todo를 지워주는 로직을
                                                                //      잘라내서 여기로 옮겨줌. 바로 윗줄의 삭제요청이 성공시에
                                                                //      이 줄이 실행된다.
      } catch (err){                                            // (10) 입력함
        console.log(err);                                       // (10) 입력함
        error.value = 'Something went wrong';                   // (10) 입력함   
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

<!-- 


-->


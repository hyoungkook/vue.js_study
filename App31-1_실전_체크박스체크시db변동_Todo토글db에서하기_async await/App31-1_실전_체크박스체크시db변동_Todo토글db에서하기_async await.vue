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
               /* (1) 지금 체크박스를 누르면 화면상에서는 표시가 되는데, db에서는 변동이 없다. 
                      지금은 다만 ref[] 에서만 체크해 주고 있는 것이라서 새로고침을 하면 다시 화면에 나타난다.     
                      그래서 체크했을때 db에도 업데이트 해주는 작업을 해보겠다.         
                  (2) json server를 보면(노트 참고) put과 patch가 있다. put과 patch는 거의 똑같이 사용할 수 있는데, 
                      의미만 조금 다르다. put은 데이터 전체를 통째로 변경해주는 것이고, patch는 부분적으로 변경해 주는 것.
                      나는 db의 completed 데이타 한개만 변경할 것이기 때문에, patch를 써보도록 하겠다. 
                      ex) PATCH "/posts/1"                                            */ 

    const toggleTodo = async (index) => {        // (3) async 입력함
      error.value = '';                          // (6) 입력함(아래 deleteTodo 함수 부분에서 복사해서 붙여넣기 했음) id가 필요하기 때문이라고 함.         
      const id = todos.value[index].id;          // (6) 입력함(아래 deleteTodo 함수 부분에서 복사해서 붙여넣기 했음) id가 필요하기 때문이라고 함.


      try {                                      // (4) try와 catch 입력함
        await axios.patch('http://localhost:3000/todos/' + id, {  
                                                 // (6) 입력함(아래 deleteTodo 함수 부분에서 복사해서 붙여넣기 했음) 
                                                 //     기존의 delete -> patch로 변경
          completed : !todos.value[index].completed
                                                 // (7) patch요청을 보내면서 데이터(토글 체크상태)도 같이 보내야 한다.     
                                                 //     그래서 completed를 보내야 하는데, (8)의 좌측이 기존값이고 우측이 변경된 값이기 때문에 completed 상태를 변경된 값으로 입력한다.                                            
        });       
         todos.value[index].completed = !todos.value[index].completed;        
                                                 // (8) catch 아래 있던 것인데 잘라서 붙여넣음.
                                                 //     데이터가 바뀌면 배열도 업데이트하는 작업.  끝.
      } catch (err) {                            // (4) try와 catch 입력함
          error.value = 'Something went wrong';  // (5) 입력함(위에서 복사해서 붙여넣기 했음)
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

   

<template>
  <div class="container">    

    <h2>To-Do List</h2>                 

    <input class="form-control" type="text" v-model="searchText" placeholder="Search">
   
    <hr />                        
    <TodoSimpleForm @add-todo="addTodo"/>   
    <div style="color:red">{{ error }}</div>    <!-- (9) 입력함 --> 
                                                <!-- (10) 여기까지하고 db서버를 죽이면 에러 메시지가 뜬다 -->
    
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
    const error = ref('');         // (5) 입력함. 에러를 처음에는 empty string으로 넣어줌. 

    const addTodo = (todo) => {      

      error.value = '';            // (6) 입력함. 만약 addTodo함수가 실행되면, 에러를 empty string으로 초기화 해주고,                                     
      axios.post('http://localhost:3000/todos', {  
        subject: todo.subject,
        completed: todo.completed,
       
        // (1) javascript에서 이런 요청(위 세줄)은 비동기적으로 일어나게 된다. 그래서 axios.post로 보내게되면 응답이 promise로
        //     오게 되는데, 비동기적으로 일어난다는 것은 요청을 보내고 응답이 오기전에 바로 그 다음으로 넘어가게 된다
        //     (여기서는 아래쪽의 todos.value.push(todo);) 즉 보낸 요청이 끝났는지 알 수 없을때, todo가 todos array에 추가가
        //     되는 것이기 때문에 데이터 베이스에 저장이 되었는지 확신할 수 없다. 
                
       }).then(res => {             // (2) 입력함. 그래서 then을 사용하면 요청이 끝나고 응답이 왔을때(데이터베이스에 저장되었을때), 
                                    //     then의 괄호안에 있는 부분이 실행된다.
           console.log(res);        //     확인용
           todos.value.push(todo);  // (3) 기존의 todos.value.push(todo); 를 잘라내기 해서 then의 괄호안에 넣어준다.
                                    // (4) push의 괄호에 todo -> res.data를 입력. (id까지 포함한 데이터가 todos array에 추가됨)
       }).catch(err => {            // (4) 입력함. 만약 데이터베이스에 저장이 안되고 실패하게 되면 이 부분이 실행됨. 
                                    //     대표적으로 데이터베이스 서버가 죽었을때 에러 메시지를 띄워보겠다. 
           console.log(err);        //     확인용
           error.value = 'Something went wrong';  // (7) 입력함. 
       });
                                                                                            
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
      error,            //(8) 입력함

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

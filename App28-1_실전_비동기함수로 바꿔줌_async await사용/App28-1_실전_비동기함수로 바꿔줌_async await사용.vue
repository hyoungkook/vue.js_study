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

    const addTodo = async (todo) => {     // (2) async 입력      
                                          // (1) 비동기적 실행 안에 비동기적 실행을 계속 넣는걸 콜백 지옥이라 불렀는데,
                                          //     사람이 보기에는 순차적으로 실행하는 것이 좋다. 
                                          //     그래서 생긴 기능이 async await이다.
                                          //     async 사용 방법은 예를들어 addTodo 함수 전체를 비동기로 만들어 버리는 것.
                                          //     27-1강 비동기적 실행개념 강의를 한번 보고 올 것.
                                                    
      error.value = '';                           
      console.log('start')                        

      try {                               // (6) 입력함. try로 묶어줌
        const res = await axios.post('http://localhost:3000/todos', {  
                                          // (4) const res = await 부분을 추가 입력함
                                          //     비동기요청 앞부분에 await를 입력하고 이 부분을 변수 res에 담음
        subject: todo.subject,                     
        completed: todo.completed,
       });
       todos.value.push(res.data);        // (5) 결과를 todos.value에 push해준다. (아래 주석에서 복사 붙여넣기함)
      } catch(err) {                      // (7) 앞쪽은 try로 묶고, 여기에 catch(err)을 입력. (아래 주석에서 복사 붙여넣기)
                                          //     여기까지 하면 아래 주석 처리한 then을 사용한 것과 똑같은 역할을 
                                          //     try에서 catch(err) 까지가 하게 됨. 
          console.log(err);        
          error.value = 'Something went wrong';   
      }

       /*.then(res => {                   // (3) 기존에 쓰던 then ~ catch 를 안 쓰는 것으로 주석 처리함  
           console.log(res);       
           todos.value.push(res.data); 
                                   
       }).catch(err => {           
                                  
           console.log(err);        
           error.value = 'Something went wrong';   
       });                                       */
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

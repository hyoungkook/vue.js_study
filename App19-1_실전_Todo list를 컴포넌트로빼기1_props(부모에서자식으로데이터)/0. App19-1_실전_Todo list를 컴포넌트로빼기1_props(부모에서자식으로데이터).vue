<template>
  <div class="container">                          
    <h2>To-Do List</h2>                              
    <TodoSimpleForm @add-todo="addTodo"/>   
    
    <div v-if="!todos.length">                   
      추가된 todo가 없습니다 
    </div>
                           <!-- (1) Todo list를 컴포넌트로 빼는 것을 해보겠다.
                                (2) components폴더에 Todolist.vue 파일 만들기
                                (3) 아래 부분을 잘라서 Todolist.vue에 붙여넣기(Todolist.vue 파일로 gogo) -->
<!--
    <div v-for="(todo, index) in todos" :key="todo.id" class="card mt-2">   
            
      <div class="card-body p-2 d-flex align-items-center" >                            
        <div class="form-check flex-grow-1">  
          <input  
            class="form-check-input" 
            type="checkbox"
            v-model="todo.completed"
            >                   
          <label class="form-check-lavel" :class="{ todo:todo.completed }">            
            {{ todo.subject }}   
          </label>        
        </div>
        <div>               
          <button 
            class="btn btn-danger btn-sm"
            @click="deleteTodo(index)"
            >Delete
          </button>           
        </div>
      </div>
    </div>                                                                                                       -->
<TodoList :todos="todos" />      <!-- (7) <TodoList /> 입력함  -->
                                 <!-- (10) 속성에 :todong="todos" 입력함
                                                :(props로보내는이름)="보내는 데이터명"   
                                           자식컴포넌트에 todos라는 이름으로 todos데이터를 보내게 됨.                -->
  </div>
</template>
    
<script>
import {ref} from 'vue';           
import TodoSimpleForm from './components/TodoSimpleForm.vue';          
import TodoList from './components/TodoList.vue';              // (5) 입력함                                                               

export default {
  components: {                               
    TodoSimpleForm,
    TodoList,                                                  // (6) 입력함                            
  },                                          
  setup() {
    const todos = ref([]);        // (9) 입력되어있던 것. 
                                  //     TodoSimpleForm에서 emit을 통해 자식콤포넌트에서 여기로
                                  //     데이터를 추가시키기 때문에, 이걸 TodoList.vue로 옮길수 없음.
                                  //     따라서 내가 할 수 있는 것은 todos 데이터를 TodoList.vue로 패스해주는 것.
                                  //     이렇게 부모컴포넌트에서 자식 컴포넌트로 데이터를 보낼때는 props를 이용한다.
    const addTodo = (todo) => {         
      todos.value.push(todo);            

    };


    const deleteTodo = (index) => {  
      todos.value.splice(index, 1);  
    };

    return {                                                       

      todos,                         
      deleteTodo, 
      addTodo,                                                                           
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
<template>
  <div class="container">    

    <h4>count: {{ count }}</h4>                 <!-- (5) 입력함 -->
    <h4>double count: {{ doubleCount }}</h4>    <!-- (5) 입력함 -->
    <button @click = "count++">Add One</button> 
           <!-- (6) 입력함(참고:template안에서는 count.value라고 할 필요 없이 바로 count라고 쓰면 된다.) -->
           <!--     버튼을 누르면 count는 1씩 증가하고, doubleCount는 count x 2로 나타남 -->
           <!--     여기까지보면 computed가 일반 함수가 다를게 없어보이는데, 함수와 다른점을 보여주겠다. -->

    <h2>To-Do List</h2>                              
    <TodoSimpleForm @add-todo="addTodo"/>   
    
    <div v-if="!todos.length">                   
      추가된 todo가 없습니다 
    </div>
                                                                              
<TodoList :todos="todos" @toggle-todo="toggleTodo" @delete-todo="deleteTodo"/>  
            
  </div>
</template>
    
<script>
import {ref,computed} from 'vue';           // (2) computed 입력함
import TodoSimpleForm from './components/TodoSimpleForm.vue';          
import TodoList from './components/TodoList4.vue';                                                                      

export default {
  components: {                               
    TodoSimpleForm,
    TodoList,                                                                          
  },                                          
  setup() {
    const todos = ref([]);      
    const addTodo = (todo) => {         
      todos.value.push(todo);            

    };

    const toggleTodo =(index) => {   
      todos.value[index].completed = !todos.value[index].completed;                               
    }

    const deleteTodo = (index) => {  
      todos.value.splice(index, 1);  
    };

    const count = ref(1);                        // (3) 입력함
    const doubleCount = computed(() => {         // (3) 입력함
      return count.value * 2                     // (3) 입력함
                                                 //     doubleCount 값에는 항상 count값의 곱하기 2값이 들어가게 된다

    });

    return {                                                       

      todos,                         
      deleteTodo, 
      addTodo,                 
      toggleTodo,             
      count,                              // (4) 입력함 (여기에 입력해야 template에서 접근이 가능해짐)
      doubleCount,                        // (4) 입력함 (여기에 입력해야 template에서 접근이 가능해짐)                           
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

(1) computed는 state가 다른 state에 의존할때 사용하는 state를 말한다.

<공식 홈페이지 예제>
const count = ref(1)
const plusOne = computed(() => count.value + 1)

plusOne이라는 값이 computed를 사용하고 있다. computed안을 보면 함수 count.value+1이라는 함수가 있는데,
count.value에 1을 더해주고 있다. (count.value는 ref괄호안에 있는 값이다)
맨처음 count 값은 1. plusOne 값은 2이다. 그러다 count값이 2로 변경 되면, plusOne도 3으로 바뀌게 된다. 



-->
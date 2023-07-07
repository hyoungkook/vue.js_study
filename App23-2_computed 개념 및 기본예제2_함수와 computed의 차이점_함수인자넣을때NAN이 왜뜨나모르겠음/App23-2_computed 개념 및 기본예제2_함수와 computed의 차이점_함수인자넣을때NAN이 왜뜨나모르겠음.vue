<template>
  <div class="container">    

    <h4>count: {{ count }}</h4>                 
    <h4>double count computed: {{ doubleCountComputed }}</h4>  
                              <!-- (4) 기존의 doubleCount => doubleCountComputed로 변경 -->
                              <!--     computed는 값이 리턴되므로 그대로 쓰면 됨 -->
    <h4>double count method: {{ doubleCountMethod() }}</h4>      
                              <!-- (5) 입력함(doubleCountMethod는 함수이기 때문에 괄호를 써줘야)-->
                              <!-- (6) 여기까지 하고 Add one 버튼 누르면 computed와 doubleCountMethod 
                                       둘다 2배씩 증가. 무슨 차이가 있나?                      -->
    <button @click = "count++">Add One</button> 

    <h2>To-Do List</h2>                              
    <TodoSimpleForm @add-todo="addTodo"/>   
    
    <div v-if="!todos.length">                   
      추가된 todo가 없습니다 
    </div>
                                                                              
<TodoList :todos="todos" @toggle-todo="toggleTodo" @delete-todo="deleteTodo"/>  
            
  </div>
</template>
    
<script>
import {ref,computed} from 'vue';          
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

    const count = ref(1);                        
    const doubleCountComputed = computed(() => { // (2) 기존의 doubleCount => doubleCountComputed로 변경
      return count.value * 2;                    // (8) 두번째 차이점. computed는 한번 계산하면 그 값을 저장하고 있다.
                                                 //     그래서 template 에서 computed를 두번 사용하게 되면, 
                                                 //     이미 그 값이 계산되어 있기 때문에, computed는 한번만 실행되고 그 값을 불러온다. 
                                                 //     반면 함수는 두번 사용하게 되면 함수를 두번 실행한다. (console.log로 확인)

    });
    const doubleCountMethod = () => {     // (1) 앞 강의에 이어서 computed가 함수와 다른점을 보여주겠다.입력함.
                                          // (7) 인자 name입력.
      return count.value * 2;             // (7) 인자 name입력(기존 입력된 2 -> name )
                                             //     함수는 이렇게 인자로 값을 받아오는데, computed는 인자로
                                             //     값을 받아올 수가 없다. computed는 이 computed함수안에
                                             //     reactive state가 있을때만, 그리고 reactive state가 변경될때만
                                             //     실행이되어 그 값을 변수에 저장하게 된다. (첫번째 차이점)
    };

    return {                                                       

      todos,                         
      deleteTodo, 
      addTodo,                 
      toggleTodo,             
      count,                           
      doubleCountComputed,                   // (3) 기존의 doubleCount => doubleCountComputed로 변경 
      doubleCountMethod,                     // (3) 입력함                                  
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

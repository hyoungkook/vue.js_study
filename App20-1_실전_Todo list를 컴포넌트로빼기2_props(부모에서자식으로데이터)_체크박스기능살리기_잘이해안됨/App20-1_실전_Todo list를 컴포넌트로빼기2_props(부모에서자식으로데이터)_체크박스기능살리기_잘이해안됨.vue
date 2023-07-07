<template>
  <div class="container">                          
    <h2>To-Do List</h2>                              
    <TodoSimpleForm @add-todo="addTodo"/>   
    
    <div v-if="!todos.length">                   
      추가된 todo가 없습니다 
    </div>
                                                                              
<TodoList :todos="todos" @toggle-todo="toggleTodo" /> <!-- (7) @toggle-todo="toggleTodo" 입력함  
                                                              (지정한 이벤트 이름)="함수명"              -->
  </div>
</template>
    
<script>
import {ref} from 'vue';           
import TodoSimpleForm from './components/TodoSimpleForm.vue';          
import TodoList from './components/TodoList2.vue';                                                                      

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

    const toggleTodo =(index) => {     // (8) 입력함. (내가 index(TodoList2.vue의 (5)번)를 보냈기 때문에 인자에는 index가 들어감)
      console.log(todos.value[index]); // (11) 입력함 (브라우저 개발자 도구에서 나타나야 하는데 안바뀌네;)
      todos.value[index].completed = !todos.value[index].completed;

                                       // (10) 입력함.
                                       //      todos를 업데이트해보겠다. ref이니까 value를 써줘야 하고, index를 받아왔으니까
                                       //      todos array에서 우리가 원하는 index를 찾아서..그다음에 completed. 
                                       //      그리고  토글을 해줄거니까 (11)을 입력하는 것을 통해서 트루였으면 false로 false였으면 true로 바꿔주는 코드를 
                                       //      적어보았다.

      console.log(todos.value[index]); // (11) 입력함 (브라우저 개발자 도구에서 나타나야 하는데 안바뀌네;)

              
                                             
    }

    const deleteTodo = (index) => {  
      todos.value.splice(index, 1);  
    };

    return {                                                       

      todos,                         
      deleteTodo, 
      addTodo,                 
      toggleTodo,                    // (9) 입력함.                                                          
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
<배경설명>

모든 props는 부모에서 자식 컴포넌트로 단방향 바인딩을 한다. 부모에서 자식으로만 데이터를 내려주는 것.

그리고 부모 컴포넌트의 property가 변경이 되면, props로 받은 데이터도 
자식 컴포넌트에서 같이 변경이 된다. (물론 반대는 아님) 자식컴포넌트에서 prop로 받은 데이터를 
직접적으로 변경하면 안된다는 것 (TodoList2.vue로 이동)

-->

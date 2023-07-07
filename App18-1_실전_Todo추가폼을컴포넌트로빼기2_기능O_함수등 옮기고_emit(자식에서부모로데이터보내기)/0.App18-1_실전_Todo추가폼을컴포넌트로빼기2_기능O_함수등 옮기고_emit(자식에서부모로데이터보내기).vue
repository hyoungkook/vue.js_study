<template>
  <div class="container">                          
    <h2>To-Do List</h2>                              
    <TodoSimpleForm @add-todo="addTodo"/>   <!--  (13) @add-todo="함수이름" 입력함 

    <form @submit.prevent="onSubmit" class="" >    (1) 왼쪽 폼태그 부분을 잘라내기 해서 TodoSimpleForm.vue의 template에 붙여넣었지
      <div class="d-flex">                             폼자체는 이렇게 해도 나타나는데 add버튼을 눌렀을때 Todo가 추가되는 기능이 사라졌음(배경)
        <div class="flex-grow-1 mr-2">                 파일 TodoSimpleForm.vue로 GO GO 
        <input                                         (파일을 서로 왔다갔다해야)
          class="form-control" 
          type="text" 
          v-model="todo"                       
          placeholder="Type new to-do">           
        </div>

        <div>                                           
          <button 
          class="btn btn-primary" 
          type="submit"> Add </button>            
        </div>

      </div>
      <div v-show="hasError" style="color:red"> This field cannot be empty</div>                            
    </form>                                                                                  -->
    
    <div v-if="!todos.length">                   
      추가된 todo가 없습니다 
    </div>

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
      </div>             
    </div>
</template>
    
<script>
import {ref} from 'vue';           
import TodoSimpleForm from './components/TodoSimpleForm.vue';                                                                          

export default {
  components: {                               
    TodoSimpleForm                            
  },                                          
  setup() {
    /* const hasError = ref(false);      // (3) hasError 여기에 있음
       const todo = ref('');             // (3) todo라는 데이터 여기에 있음
                                         // (4) 여기 있는 hasError와 todo를 잘라내서 TodoSimpleForm으로 옮기기 */
    const todos = ref([]);            

/*
    const onSubmit = () => {             // (3) onSubmit 함수 여기에 있음
      if (todo.value === '') {           // (7) 여기 있는 onSubmit 함수를 잘라내서 TodoSimpleForm으로 옮기기
        hasError.value = true;     
      } else {                  
        todos.value.push({             
        id:Date.now(),               
        subject:todo.value,
        completed:false,     
       });
        hasError.value = false;    
        todo.value="";        
      }
    };                                                 */

    const addTodo = (todo) => {           // (14) 입력함
                                          // (16) 자식컴포넌트에서 받아온 인자를(todo) 데이터로 넣어준다.
      todos.value.push(todo);             // (17) 입력함(자식컴포넌트로부터 전달된 todo를 todos array에 넣는다. 끝)

    };


    const deleteTodo = (index) => {  
      todos.value.splice(index, 1);  
    };

    return {                                                       
     // todo,                             // (6) 삭제함
      todos,                         
     // onSubmit,                         // (7) 삭제함
     // hasError,                         // (6) 삭제함
      deleteTodo, 
      addTodo,                            // (15) 입력함                                                   
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
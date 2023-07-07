<template>
  <div class="container">                          
    <h2>To-Do List</h2>                              
    <TodoSimpleForm />                       <!-- (5) 입력함(이거 한줄만) 

    <form @submit.prevent="onSubmit" class="" >   (6) 왼쪽 폼태그 부분을 잘라내기 해서 TodoSimpleForm.vue의 template에 붙여넣기
      <div class="d-flex">                           폼자체는 이렇게 해도 나타나는데 add버튼을 눌렀을때 Todo가 추가되는 기능이 사라졌음
        <div class="flex-grow-1 mr-2">                    
        <input                                         
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
import TodoSimpleForm from '../components/TodoSimpleForm.vue'; 
                            // (1) Todo 추가폼을 컴포넌트로 빼서 가져다 쓰고 싶다. 
                            // (2) 폴더에 파일추가 src>components>TodoSimpleForm.vue
                            // (3) 입력함. TodoSimpleForm이라는 이름은 원하는 걸 쓰면 되나, 
                            //     보통 컴포넌트 파일 이름을 쓰는 것이 알아보기도 쉽고 좋다.
                                                                                    

export default {
components: {                              // (4) 컴포넌트를 등록함
TodoSimpleForm                             // (4) 컴포넌트를 등록함
},                                         // (4) 컴포넌트를 등록함
  setup() {
    const hasError = ref(false);   
    const todo = ref('');                         
    const todos = ref([]);            

    const onSubmit = () => {          
      if (todo.value === '') {     
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
    };

    const deleteTodo = (index) => {  
      todos.value.splice(index, 1);  
    };

    return {                                                       
      todo,
      todos,                         
      onSubmit,
      hasError, 
      deleteTodo,                                                    
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
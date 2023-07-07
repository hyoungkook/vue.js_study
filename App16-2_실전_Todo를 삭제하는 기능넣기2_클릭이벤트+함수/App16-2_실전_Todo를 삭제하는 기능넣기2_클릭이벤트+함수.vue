<template>
  <div class="container">                          
    <h2>To-Do List</h2>                              
    <form @submit.prevent="onSubmit" class="" > 

      <div class="d-flex">              
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
                                      
    </form>
                                            
    <div v-for="(todo, index) in todos" :key="todo.id" class="card mt-2">   
                            <!-- (5) "todo in todos"였는데 => "(todo, index) in todos" 로 입력 -->

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
            </button>           <!-- (1) 삭제버튼을 눌렀을때 todo리스트 삭제 기능 넣기 -->
                                <!-- (2) @click="deleteTodo" 입력함(deleteTodo라는 함수이름은 만들어 낸것)-->
                                <!-- (6) (index) 입력함 -->
          </div>
        </div>
      </div>             
    </div>
</template>
    
<script>
import {ref} from 'vue';                                                  

export default {
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

    const deleteTodo = (index) => {  // (3) 기본 함수 입력함
                                     // (7) index를 입력해서 index값을 받아옴
      todos.value.splice(index, 1);  // (8) todos에 value array에다 splice를 이용해서 
                                     //    index 그리고 1개만 지워주는 로직을 입력
    };

    return {                                                       
      todo,
      todos,                         
      onSubmit,
      hasError, 
      deleteTodo,                // (4) 입력함                                     
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
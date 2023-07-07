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
    
    <div v-if="!todos.length">                   
                            <!-- (1) Todo리스트에 아무것도 없을때 메시지가 나타나게 하고 싶다. div태그 입력함-->
                            <!-- (3) v-if="!todos.length" 입력함. 
                                    (todos의 array에 length가 없으면, 사라지게 만들어 보았다) (끝)         -->
      추가된 todo가 없습니다 <!-- (2) 입력함. 여기까지 하면 메시지가 뜨는데, todo가 추가되어도 
                                    메시지가 사라지지 않음 -->

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
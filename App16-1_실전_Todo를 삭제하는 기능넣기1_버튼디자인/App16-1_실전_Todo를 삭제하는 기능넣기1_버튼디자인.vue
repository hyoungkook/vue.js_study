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
                                            
    <div v-for="todo in todos" :key="todo.id" class="card mt-2">   

      <div class="card-body p-2 d-flex align-items-center" >     
                                              <!-- (5) d-flex 추가 (버튼이 줄이 바뀌지 않네)       -->
                                              <!-- (7) align-items-center 추가 (텍스트가 약간 위쪽에 있었는데 중앙으로 내려온다는데 안내려오네;) -->
        <div class="form-check flex-grow-1">  <!-- (6) flex-grow-1 추가(버튼이 오른쪽 끝으로 정렬) -->
          <input 
            class="form-check-input" 
            type="checkbox"
            v-model="todo.completed"
            >                   
          <label class="form-check-lavel" :class="{ todo:todo.completed }">            
            {{ todo.subject }}   
          </label>        
        </div>
          <div>                    <!-- (1) To-do list 삭제버튼을 넣고 싶다. div태그를 만듬       -->
            <button class="btn btn-danger btn-sm">Delete</button>    
                                   <!-- (2) 버튼을 넣어준다.                                     -->
                                   <!-- (3) class="btn btn-danger" 부트스트랩 버튼디자인(붉은 바탕 흰글씨) -->
                                   <!-- (4) btn-sm 이어서 입력함. 부트스트랩 버튼디자인(버튼이 작아짐)      -->
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

    return {                                                       
      todo,
      todos,                         
      onSubmit,
      hasError,                                      
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
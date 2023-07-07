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
      <div class="card-body p-2" >  

      <div class="form-check"> 
        <input 
          class="form-check-input" 
          type="checkbox"
          v-model="todo.completed"
          >                   
        <label class="form-check-lavel" :style="todo.completed ? todoStyle : {}">        
      <!-- (1) 체크박스 체크시 밑줄이 그어지면서 글자색을 회색으로 하고 싶다. 
              :style="todoStyle" 입력함. 
           (4) 따라서 스타일 바인딩에서 위의 todo.completed에 접근을 해야. 
           (5) :style="todo.completed ? todoStyle : {}" 을 입력해서 스타일 바인딩을 완성.
               (todo가 completed일때 todostyle을 적용하고, todo가 completed가 아닐때는 빈 object를 넣어준 문법이라는 뜻임)    -->        
                                 
          {{ todo.subject }}   
        </label>        
      
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
    const todoStyle = {             // (2) 입력함. style바인딩방법중에는 object를 이용하는 방법도 있고, array를 이용하는 방법도 있는데, 여기서는 object를 이용해서 하는 방법으로 한다.
                                    //     css에서는 text-decoration으로 해주는데, javascript에선 textDecoration.
      textDecoration:'line-through',
      color:'gray'
    }

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
      todoStyle,                    // (3) 입력함. 여기까지 하면 체크박스 체크시에만 줄이 그어지면 좋겠는데, 추가되는 모든 todo에 줄이 그어짐     
  };
}
}
</script>

<style>
 .name{
   color : blueviolet;
 }
</style>
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
        <label class="form-check-lavel" :class="{ todo:todo.completed }">        
               <!-- (2) :class="{ todo:todo.completed }" 입력함.
                       공식은 <div :class="{active:isActive}"></div>
                        1> active에 넣고 싶은 class이름을 넣음
                        2> isActive에는 불리안이 들어감. isActive가 true면 active에 있는 class가 추가가 되고,
                           isActive가 false면 Active에 있는 class가 추가되지 않음.
               -->     
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
 .todo{                                 /* (1) 입력함   */
   color : gray;
   text-decoration:line-through;
 }
</style>
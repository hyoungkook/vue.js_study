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

      <div class="form-check">  <!-- (1) 체크박스 넣기 시작. div태그 만들고, class="form-check" 넣기(부트스트랩) -->
        <input 
          class="form-check-input" 
          type="checkbox"
          v-model="todo.completed"
          >                     <!-- (2) input 입력함 (클래스는 디자인) -->     
                                <!-- (6) (5)에서 입력한 completed를 v-model을 통해 바인딩해줌  
                                         completed의 기본값이 false이기 때문에 todo를 추가했을때 체크가 안된 것으로 나타남.
                                         completed의 기본값을 true로 해주면 todo를 추가했을때 체크된 것으로 나타남. -->
        <label class="form-check-lavel">  
                                <!-- (3) 입력함 (클래스는 디자인) -->
          {{ todo.subject }}    <!-- (4) 바로 아래 div태그안에 있던 걸 label안으로 옮겨줌 -->
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
        completed:false,      // (5) 처음 todo를 추가할때, 당연히 완료가 안되어 있어야 하므로 기본으로 false를 넣어줌.
                              //     (true로 해주면 체크박스가 체크된 상태로 나타남)
                              //     (id와 subject를 추가했는데, 이렇게 한가지를 더 추가함)
       });
        hasError.value = false;    
        todo.value="";        // (7) 추가버튼을 눌러서 todo를 추가하고 나면 input박스에 입력한 내용은 지워주는 기능
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
 .name{
   color : blueviolet;
 }
</style>
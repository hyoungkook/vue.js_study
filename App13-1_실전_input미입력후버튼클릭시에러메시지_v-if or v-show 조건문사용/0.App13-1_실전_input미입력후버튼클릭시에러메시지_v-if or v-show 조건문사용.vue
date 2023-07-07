<template>
  <div class="container">                          
    <h2>To-Do List</h2>                              
    <form @submit.prevent="onSubmit"> 

      <div class="d-flex">              <!-- (7) div태그를 만들어서 input과 button태그를 감싸줌(디자인위해) -->
                                        <!-- (8) 바로위 form태그에 있던 class="d-flex"를 옮겨옴(이렇게 해야 Error메시지가 버튼 밑으로 내려감) --> 
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
                                          <!-- (3) 입력함
                                                  v-show로 인해서 hasError의 값이 false일때 에러메시지가 뜨고, hasError의 값이 true일때 에러메시지가 안뜸
                                                                                                                        --> 
                                          <!-- (9) v-if를 v-show로 바꿔도 그대로 실행됨.
                                                  v-if : 조건이 거의 안바뀔때 사용 (초기 랜더 비용 낮음. 조건에 맞는 것만 표시)
                                                  v-show : 조건이 자주 바뀔때 사용 (초기 랜더 비용 많이 듬. 일단 다 불러옴) -->
    </form>
                                            
    <div v-for="todo in todos" :key="todo.id" class="card mt-2">                       
      <div class="card-body p-2" >  
        {{ todo.subject }}               
      </div>
    </div>             
  </div>
</template>
    
<script>
import {ref} from 'vue';                                                  

export default {
  setup() {
    const hasError = ref(false);   /* (1) input박스에 아무것도 입력안하고 버튼 클릭시 에러메시지 뜨도록 하고싶음
                                          (제대로 입력하면 에러메시지가 안뜨도록)
                                          hasError이 false일때는 false div태그를 보여주고, true일때는 
                                          true div태그를 보여주고 싶다. 이럴때 사용하는 것이 v-show나 v-if. */
    const todo = ref('');                         
    const todos = ref([
      {id:1, subject:'휴대폰 사기'}, 
      {id:2, subject:'장보기'},     
    ]);            

    const onSubmit = () => {          
      if (todo.value === '') {     // (4) 입력함. todo value가 공란일때 hasError의 value가 true로 바뀌도록
        hasError.value = true;     // (4) 입력함. todo value가 공란일때 hasError의 value가 true로 바뀌도록
      } else {                     // (5) 입력함. 그렇지 않을 경우(기존에 입력되어 있던 것)는 else안에 넣어줌.
        todos.value.push({             
        id:Date.now(),               
        subject:todo.value            
       });
        hasError.value = false;    // (6) 입력함.(이렇게 해야 잘 추가되었을 경우 에러메시지가 뜨지 않음)
      }
     
    };

    return {                                                       
      todo,
      todos,                         
      onSubmit,
      hasError,                    // (2) 입력함
  };
}
}
</script>

<style>
 .name{
   color : blueviolet;
 }
</style>
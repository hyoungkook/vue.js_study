<template>
  <div class="container">                          
    <h2>To-Do List</h2>                              
    <form @submit.prevent="onSubmit" class="d-flex" > 
                           
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

    </form>
    {{todos}}                    
    
    <div class="card">              <!-- (1) 부트스트랩의 card라는 컴포넌트.(제출을 눌렀을때 카드가 뜨도록 하는 디자인) -->
      <div class="card-body p-2" >  <!-- (2) 카드내 글자와 테두리의 간격을 조정함. 패딩(p-2) -->
        {{ todos[0].subject }}      <!-- (4) (3)에서 입력한 데이터를 이렇게 끌고와서 카드에 표시함 -->
      </div>
    </div>

   <div class="card">               <!-- (5) 이 div태그 전체를 복사/붙여넣기 하여 카드를 한장 더 만들어본다. -->
      <div class="card-body p-2" > 
        {{ todos[1].subject }}      <!-- (6) 숫자를 0대신 1로 바꿔서 (3)에서 입력한 다른 데이터를 끌고와서 카드에 표시 -->
      </div>
   </div>
   
  </div>
</template>
    
<script>
import {ref} from 'vue';                                                  

export default {
  setup() {
    const todo = ref('');                         
    const todos = ref([
      {id:1, subject:'휴대폰 사기'},  // (3) 카드내에 들어갈 수 있는 데이터를 넣어줌 
      {id:2, subject:'장보기'},       // (3) 카드내에 들어갈 수 있는 데이터를 넣어줌
    ]);            

    const onSubmit = () => {          
      todos.value.push({             
        id:Date.now(),               
        subject:todo.value            
      });
    };

    return {                                                       
      todo,
      todos,                         
      onSubmit,
  };
}
}
</script>

<style>
 .name{
   color : blueviolet;
 }
</style>
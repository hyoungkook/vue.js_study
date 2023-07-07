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
                                            
    
    <div v-for="todo in todos" :key="todo.id" class="card mt-2">  
                       <!--(1) 반복되는 부분 최상위 태그에 v-for를 넣는다.              
                               그리고 접근하고 싶은 array를 넣어서 v-for="todos"를 쓰고
                               todos라는 array에서 하나하나 빼온 값을 담을 건데, 
                               v-for="todo in todos"라고 쓴다(todo는 아무 이름으로 해도 됨)  -->
                       <!--(4) v-for를 사용할때는 key값을 필수로 넣어줘야 함.    
                               유니크한 값을 넣어야해서 todo에 있는 id를 넣었음.          
                               vue.js에서 각각의 노드들을 추적하기 위해 key를 사용.           -->      
      <div class="card-body p-2" >  
        {{ todo.subject }}  <!--(2) todos[0]이었는데 todo로 접근이 되니까 todo로 하면 됨     -->
      </div>
    </div>
    
                            <!--(3) 반복되는 두번째 카드가 있었는데 삭제함                   --> 

  </div>
</template>
    
<script>
import {ref} from 'vue';                                                  

export default {
  setup() {
    const todo = ref('');                         
    const todos = ref([
      {id:1, subject:'휴대폰 사기'}, 
      {id:2, subject:'장보기'},     
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
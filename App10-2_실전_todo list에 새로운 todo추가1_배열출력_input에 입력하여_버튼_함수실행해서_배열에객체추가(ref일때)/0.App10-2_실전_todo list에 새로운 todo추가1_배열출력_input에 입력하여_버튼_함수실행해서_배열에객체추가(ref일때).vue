<template>
  <div class="container">                          
    <h2>To-Do List</h2>                              
    <form @submit.prevent="onSubmit" class="d-flex" > 
                                      <!-- (9) 이 form 태그안의 button 태그에 있던 @click="onsubmit"을 옮겨왔음.
                                               그대로 사용하면 input을 클릭만해도 empty array가 뜨기 때문에 @submit="onSubmit"으로 사용 
                                               input박스에 입력후 엔터를 치든 버튼을 누르든 제출되는 효과. 
                                           (10)그런데 제출했을때 화면이 리로드됨(화면에 id뜨고 지저분. form의 특성). 
                                               그래서 @submit뒤에 .prevent를 통해 리로드를 방지한다. -->
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
         type="submit"> Add </button> <!-- (8) 여기있던 @click="onSubmit"을 위의 form 태그로 옮기고 type="submit"추가함
                                               form안의 submit버튼이 눌러졌을때 onsubmit 함수가 실행됨             -->    
      </div>

    </form>
    {{todos}}                         <!-- (3) 임시로 출력하기 위해 넣음. 일단 화면상으로는 empty array가 출력되는 상태-->
  </div>
</template>
    
<script>
import {ref} from 'vue';                                                  

export default {
  setup() {
    const todo = ref('');                         
    const todos = ref([]);             // (1) list를 만들어보겠다. list니까 배열(array)을 넣는다.

    const onSubmit = () => {           // (4) add버튼을 누르면 onSubmit함수안에서 todos 배열 안으로 todo를 추가시켜 보겠다
      todos.value.push({               // (5) todos라는 배열에 push를 통해서 객체를 추가. ref이기 때문에 value가 필요.
        id:Date.now(),                 // (6) id는 유니크해야 하기 때문에 Date.now()를 통해서 타임스탬프를 넣었음
        subject:todo.value             // (7) todo의 제목은 todo.value가 들어가게 된다.
      });
    };

    return {                                                       
      todo,
      todos,                          // (2) 입력함
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
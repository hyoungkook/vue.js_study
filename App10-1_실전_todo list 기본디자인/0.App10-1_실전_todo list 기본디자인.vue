<template>
  <div class="container">                             <!-- (3) div태그로 전체 코드를 감싸줌, class="container"를 설정(부트스트랩 class. 왼쪽이랑 오른쪽을 좀 띄어줌)-->
    <h2>To-Do List</h2>                               <!-- (1) 제목 작성 -->
    <form class="d-flex" >                            <!-- (2) form태그안에 input, button 로직들을 넣는다. 
                                                           (4) class="d-flex" 입력.                                         -->
                                                            
      <div class="flex-grow-1 mr-2">                  <!-- (5) div 태그로 input을 감싸줌 -->
                                                      <!-- (6) mr-2(마진)를 입력해서 input박스와 버튼사이에 간격을 줌 (디자인) -->
                                                      <!-- (7) class = flex-grow-1 입력.(디자인)-->
       <input                                         
         class="form-control" 
         type="text" 
         v-model="todo"                       
         placeholder="Type new to-do">                <!-- (8) input에 class="form-control" 입력.(가로공간 전부 차지하도록) -->
                                                      <!-- (10)placeholder="type new to-do" 를 넣어서 input박스안에 뜨도록 함 -->
                                                      <!-- (11)v-model="변수" 에서 변수이름을 todo로 넣음(아래 ref부분에도 변수를 todo로 넣어야) -->
      </div>

      <div>                                           <!-- (6) div 태그로 button을 감싸줌 -->
        <button 
         class="btn btn-primary" 
         @click="onSubmit"> Add </button>                 
      </div>

    </form>
  
  </div>
</template>
    
<script>
import {ref} from 'vue';                                                  

export default {
  setup() {
    const todo = ref('');                           // (9) 처음시작할때는 empty string으로 넣었음(input박스에 아무것도 없도록). 
                                                    // (10)변수이름을 todo로 넣어줌(위 v-model="변수" 부분과 일치시킴)
   

    const onSubmit = () => {
      console.log(todo.value)                       // 양방향 데이터 바인딩 확인용?
    
    };

    return {                                                       
      todo,                                         // (11) 입력함
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
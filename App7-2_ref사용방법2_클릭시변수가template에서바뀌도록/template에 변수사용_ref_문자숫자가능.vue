<template>

<nav class="navbar bg-dark" data-bs-theme="dark">
  <div class="container-fluid">
    <a class="navbar-brand">Daejoo</a>
  </div>
</nav><br><br>

<div class="text-center">
<button class="btn btn-primary text-center" type="button" @click="welcome">출근인원check! </button>                                                        
</div>                                               

<br><br>
<div class="title"> {{title}} </div><br><br>                    <!-- ref사용 (1) 콧수염 괄호안에 변수입력 -->               

<div>
  <table align="center">                          
    <thead>
      <tr>
        <th>날짜</th>
        <th>이름</th>
        <th>출근여부</th>
      </tr>
    </thead>
    <tbody>
      <tr :key="i" v-for="(product,i) in productList">
        <td>{{product.day}}</td>
        <td>{{product.name}}</td>
        <td>{{product.work}}</td>
      </tr>
    </tbody> 
  </table>
</div>

</template>

<script>
import {ref} from 'vue';                                         // ref사용 (4) ref를 쓸수 있도록 import 해줌

export default {
 setup(){

let title = ref('어서 오세요 환영합니다');                         // ref사용 (2) 변수선언         

const welcome = () => {                                 
    alert("반갑습니다! 대주 북항 웹앱에 오신것을 환영합니다");
    title.value = '<북항지사 인원현황>';                          // ref사용 (5) ref로 감싸준 변수를 변하도록 
                                                                 //            클릭이벤트 -> 함수실행 -> 변수 변경



  };

return {                                              
  welcome,                                             
   productList: [
        {"day":"4월12일","name":"김형국","work":"출근"},
        {"day":"4월12일","name":"김진아","work":"출근"},
        {"day":"4월12일","name":"김호범","work":"출근"},
        {"day":"4월12일","name":"송주용","work":"휴무"},
      ],
  title,                                                        // ref사용(3) return에 변수 입력
 }
 }
}

</script>

<style>

  table{
    border-collapse:collapse;       
    width:50%;                      
  }

  th{                               
    height:20px;
    text-align:center;                
    padding:10px;                    
    border-bottom:1px solid #ddd;  
    
  }

  td{                               
    height:20px;
    text-align:center;                 
    padding:10px;                   
    border-bottom:1px solid #ddd;  
  }

.title{
  text-align:center;
  font-size:30px;
  color : blue;
}
      
</style>

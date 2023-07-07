<template>

<nav class="navbar bg-dark" data-bs-theme="dark">
  <div class="container-fluid">
    <a class="navbar-brand">Daejoo</a>
  </div>
</nav><br><br>

<div class="text-center">
<button class="btn btn-primary text-center" type="button" @click="welcome">출근인원check! </button>                                                        
</div>                                                <!-- 클릭이벤트(1) 클릭시 함수가 실행되게 만듬-->

<br><br>
<div class="title"> {{title}} </div><br><br>                             

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
import {ref} from 'vue';                                       

export default {
 setup(){

const worker = ref('홍길동');

let title = ref('어서 오세요 환영합니다');                           

const welcome = () => {                                 // 클릭이벤트(2) 셋업펑션 안에 함수를 정의하고
    alert("반갑습니다! 대주 북항 웹앱에 오신것을 환영합니다");
    title.value = '<북항지사 인원현황>';                             
                                                               



  };

return {                                              
  welcome,                                              // 클릭이벤트 (3) 함수를 리턴값으로 넣어준다.
   productList: [
        {"day":"4월12일","name":"김형국","work":"출근"},
        {"day":"4월12일","name":"김진아","work":"출근"},
        {"day":"4월12일","name":"김호범","work":"출근"},
        {"day":"4월12일","name":"송주용","work":"휴무"},
      ],
  title,                                                          
  worker,
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

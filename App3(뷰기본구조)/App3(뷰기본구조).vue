<template>
  <div class="name">{{name}}</div>   <!-- template에서 접근할때는 콧수염 괄호를 하고 안에 원하는 변수명을 넣어준다. 
                                          style에 적용하기 위해 class이름도 넣었음 -->
</template>




<script>
export default {                                                       
  setup() {
    const name = 'Kossie Coder1';    // 컴포지션 api는 이렇게 setup 펑션 안에 로직을 넣어주면 됨.(여기서는 변수를 선언했네)

    return {                         /* setup 펑션 안에서 return을 하는데, 원하는 변수를 return해주면 이 안에 있는 변수들은 
                                        template안에서 접근이 가능해짐. */
      name
    };
  }
}
</script>



<style>
 .name{                               /* template에 적용하기 위해 클래스를 설정해서 입력함 */ 
   color : red;
 }
</style>

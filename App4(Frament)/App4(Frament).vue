
<template>
    <div>              <!-- vue.js 2에서는 이렇게 쓸모가 없어도 div태그 같은걸로 한번 더 감싸줘야 했었음. vue.js 3에서는 안 그래도 됨. -->
        
        <div class="name">
          {{name}}
        </div>              
        
        <div>
          Hi
        </div>

    </div>
</template>

<script>
export default {                                                       
  setup() {
    const name = 'Kossie Coder1';               

    return {                                   
      name
    };
  }
}
</script>

<style>
 .name{                                         
   color : red;
 }
</style>
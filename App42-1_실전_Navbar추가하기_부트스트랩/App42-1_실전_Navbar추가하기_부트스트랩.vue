<template>
<!-- (2) 부트스트랩 홈페이지 Navbar 섹션으로 가서 코드를 복사하여 아래와 같이 붙여넣고, 
         필요에 따라서 사용하지 않는 버튼 코드 들을 지워준다.                                    -->

<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <a class="navbar-brand" href="#">Daejoo-industry</a> <!-- (4) 브랜드 이름 변경               -->
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>

  <div class="collapse navbar-collapse" id="navbarSupportedContent">
    <ul class="navbar-nav mr-auto">
      <li class="nav-item active">
        <a class="nav-link" href="#">Todos <span class="sr-only">(current)</span></a> 
                                                <!-- (6) 버튼 이름을 Home -> todos로 변경            -->
      </li>
<!--  <li class="nav-item">                          (5) 필요없는 코드들 삭제 (Navbar의 다른 메뉴들)            
        <a class="nav-link" href="#">Link</a>
      </li>
      <li class="nav-item dropdown">
        <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
          Dropdown
        </a>
        <div class="dropdown-menu" aria-labelledby="navbarDropdown">
          <a class="dropdown-item" href="#">Action</a>
          <a class="dropdown-item" href="#">Another action</a>
          <div class="dropdown-divider"></div>
          <a class="dropdown-item" href="#">Something else here</a>
        </div>
      </li>
      <li class="nav-item">
        <a class="nav-link disabled" href="#">Disabled</a>
      </li>                                                              -->
    </ul>
<!--<form class="form-inline my-2 my-lg-0">          (3) 삭제 (search 버튼)        
      <input class="form-control mr-sm-2" type="search" placeholder="Search" aria-label="Search">
      <button class="btn btn-outline-success my-2 my-sm-0" type="submit">Search</button>
    </form>                 -->
  </div>
</nav>

  <router-view/> <!--(1)이 부분에 페이지 컴포넌트가 들어간다. 
                        Home 컴포넌트와 Todos컴포넌트가 이 부분으로 들어감
                        그래서 <router-view>만 페이지 컴포넌트로 바뀌고, 이 윗부분은 항상 있게 된다.
                        So, 이 윗쪽에 Navbar를 넣으면 어떤 페이지로 이동하든지 항상 존재하게 된다. -->
</template>

<script>                                                                                                 
export default {                                                         
  }              
</script>

<style>
 .todo{                                
   color : gray;
   text-decoration:line-through;
 }
</style>
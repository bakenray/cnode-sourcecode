<template>
  <div id="app">
    <Header></Header>
    <div class="main">
      <!-- <Postlist></Postlist> -->
      <router-view name="slidebar" class="sidebar"></router-view>
      <router-view name="main"></router-view>
    </div>
    <Footer></Footer>
  </div>
</template>

<script>

import Header from './components/Header'
import Postlist from './components/Postlist'
import Footer from './components/Footer'
export default {
  name: 'App',
  components:{
    Header,
    Postlist,
    Footer
  }
}
</script>

<style>
body{
  margin:0;
  padding:0;
  background-color: rgb(225,225,225);
  font-size:16px;
}
ul{
  list-style: none;
  margin:0;
  padding:0;
}
a{
  text-decoration: none;
  text-shadow:none;
  color: #778087;
}
img{
  max-width: 100%;  
}
.loading{
    position: fixed;
    top: 50%;
    left: 50%;
    margin: -15px 0 15px 0;
}
.main{
    width: 80%;
    margin: 20px auto;
    overflow: hidden;
}
.topbar{
    padding:10px;
    background-color: #f6f6f6;
}
.bg-default{
  background:#fff;
  border-radius: 3px;
  overflow: hidden;
}
.loading img{
  width:80px;
  height:80px;
}

@media(max-width:768px){
.main {
    width: 98%;
}
.sidebar{
  display: none;
}
}

</style>

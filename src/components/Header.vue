<template>
 <div class="header">
     <div class="header-inner">
         <router-link :to="{name:'root'}">
            <img src="../assets/logo.svg" alt="logo">
         </router-link>
        <!-- <ul>
            <li><router-link :to="{name:'root'}">首页</router-link></li>
            <li><router-link :to="{name:'getstart'}">新手入门</router-link></li>
            <li><router-link :to="{name:'api'}">API</router-link></li>
            <li><router-link :to="{name:'login'}">登录</router-link></li>  
            <li><router-link :to="{name:'register'}">注册</router-link></li>                      
        </ul> -->
     </div>
 </div>
</template>

<script>
export default {
}
</script>

<style scoped>
.header{
    background-color:#444;
    height:50px;
}
.header-inner{
    width:90%;
    margin:auto;
    padding:5px;
}
img{
    max-width:120px;
    padding: 5px 12px;
}
ul{
    float: right;;
}
li{
    display: inline-block;
    padding:6px 10px;
}
a{
      font-size: 14px;
      color:#ccc;
      display: inline-block;
      width: 115px;
}
a:hover{
      color:#fff;
}

@media(max-width:768px){
.header{
    background-color:#444;
    height:auto;
}

a{
      font-size: 14px;
      color:#ccc;
      display:block;
      text-align: center;
      margin: auto;
      width: 50%;
}
ul{
    float: none;
}
}

</style>

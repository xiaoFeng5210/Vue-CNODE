<template>
   <div class="header">
       <img src="../assets/cnodejs_light.svg" alt="">
       <ul>
           <li><a href="#">首页</a></li>
           <li><a href="#">新手入门</a></li>
           <li><a href="#">API</a></li>
           <li><a href="#">关于</a></li>
           <li><a href="#">注册</a></li>
           <li><a href="#">登录</a></li>
       </ul>
   </div>

</template>


<script>
export default {
    
}
</script>


<style scoped>
.header{
    background-color: #5a5555;
    

}
img{
    max-width:120px;
    margin-left: 50px;
    margin-top:10px;
    padding-bottom: 8px;
}
ul>li{
    list-style: none;
    display: inline-block;
    padding-right: 20px;
    
    
    
}
ul{
    float:right;
    padding:15px 20px;
}
a{
    text-decoration: none;
    color:azure;
}
</style>

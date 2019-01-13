<template>
   <div class="left-nav">
      <ul>
          <li>
             <i class="icon iconfont icon-VIP"></i> 
             <div>会员</div>
          </li>
          <li>
             <i class="icon iconfont icon-shangpu"></i> 
             <div>商铺</div>
          </li>
          <li>
             <i class="icon iconfont icon-money"></i> 
             <div>钞票</div>
          </li>
          <li>
             <i class="icon iconfont icon-zuanshi"></i> 
             <div>钻石</div>
          </li>
          <li>
              <i class='icon iconfont icon-shouyin1'></i>
              <div>收银</div>
          </li>
          <li>
              <i class='icon iconfont icon-user'></i>
              <div>用户</div>
          </li>
      </ul> 
   </div> 
</template>


<script>

export default {
    name:'leftNav',
    data(){
        return {
            
        }
    }
}
</script>

<style>
.left-nav{
    width: 5%;
    height: 100%;
    color:#fff;
    background-color: #1D8ce0;
    float: left;
}
.left-nav ul{
    padding: 0;
    margin: 0
}
.left-nav ul li{
    list-style: none;
    text-align: center;
    border-bottom:1px solid #20a0ff;
    padding:10px;
}
.left-nav ul li div{
    font-size: 12px;
}
.iconfont{
    font-size: 24px;
}
</style>


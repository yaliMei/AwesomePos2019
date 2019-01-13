<template>
   <div class="pos">
      <el-row class="clearfix">
          <el-col :span='7' class="order-list" id="order-list">
             <div>
               <el-tabs>
                    <el-tab-pane label="点餐">
                       <el-table :data="tableData" border style="width: 100%">
                         <el-table-column prop="goodsName" label="商品名称">
                         </el-table-column>
                         <el-table-column prop="count" label="数量" width='50'>
                         </el-table-column>
                         <el-table-column prop="price" label="金额" width='70'>
                         </el-table-column>
                         <el-table-column label="操作" width='100' fixed='right'>
                             <template scope="scope">
                                 <el-button type='text' size='small' @click='delSingleGodds(scope.row)'>删除</el-button>
                                 <el-button type='text' size='small' @click='addOrderList(scope.row)'>添加</el-button>
                             </template> 
                         </el-table-column>
                      </el-table>
                      <div class="collect">
                         <small>数量：</small> {{totalCount}}&nbsp;&nbsp;&nbsp;&nbsp;<small>金额：</small>{{totalMoney}}元

                      </div>
                      <div class='div-btn'>
                         <el-button type='warning'>挂单</el-button> 
                         <el-button type='danger' @click='delAllGoods()'>删除</el-button> 
                         <el-button type='success' @click="checkout()">结账</el-button> 
                      </div> 
                    </el-tab-pane>
                    <el-tab-pane label="挂单">挂单</el-tab-pane>
                    <el-tab-pane label="外卖">外卖</el-tab-pane>
                </el-tabs>
            </div> 
          </el-col>
          <el-col :span='17' class="goods-list">
              <!-- 常用商品 -->
              <div class="often-goods">
                <div class="title">常用商品</div>
                <div class="often-goods-list">
                   <ul>
                       <li v-for="goods in oftenGoods" @click='addOrderList(goods)'>
                           <span>{{goods.goodsName}}</span>
                           <span class="price">¥{{goods.price}}元</span>
                       </li>
                   </ul> 
                </div>
              </div>
              <!-- 商品分类 -->
              <div class="type-goods">
                <el-tabs>
                    <el-tab-pane label="汉堡">
                       <div class="cookList">
                           <ul>
                               <li v-for="goods in type0Goods" @click='addOrderList(goods)'>
                                   <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                                   <span class="foodName">{{goods.goodsName}}</span>
                                   <span class="foodPrice">¥{{goods.price}}元</span>
                               </li>
                           </ul>
                       </div> 
                    </el-tab-pane>
                    <el-tab-pane label="小食">
                       <div class="smallFoods">
                           <ul>
                               <li v-for="goods in type1Goods" @click='addOrderList(goods)'>
                                   <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                                   <span class="foodName">{{goods.goodsName}}</span>
                                   <span class="foodPrice">¥{{goods.price}}元</span>
                               </li>
                           </ul>
                       </div>
                    </el-tab-pane>
                    <el-tab-pane label="饮料">
                      <div class="drinks">
                           <ul>
                               <li v-for="goods in type2Goods" @click='addOrderList(goods)'>
                                   <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                                   <span class="foodName">{{goods.goodsName}}</span>
                                   <span class="foodPrice">¥{{goods.price}}元</span>
                               </li>
                           </ul>
                       </div>
                    </el-tab-pane>
                    <el-tab-pane label="套餐">
                        <div class="setMeal">
                           <ul>
                               <li v-for="goods in type3Goods" @click='addOrderList(goods)'>
                                   <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                                   <span class="foodName">{{goods.goodsName}}</span>
                                   <span class="foodPrice">¥{{goods.price}}元</span>
                               </li>
                           </ul>
                       </div>
                    </el-tab-pane>
                </el-tabs>
              </div>
          </el-col>
      </el-row>
   </div> 
</template>


<script>
import axios from 'axios'
export default {
   name:'pos',
   data(){
       return {
      tableData:[],
      oftenGoods:[],
      type0Goods:[],
      type1Goods:[],
      type2Goods:[],
      type3Goods:[],
      totalMoney:0,
      totalCount:0
       }  
   },
   //创建时就有数据
   created:function(){
       axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods')
       .then(response=>{
        console.log(response)
        this.oftenGoods=response.data
       })
       .catch(error=>{
           console.log('您的访问数据不存在！')
       })

       axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods')
       .then(response=>{
            // console.log(response)
            this.type0Goods=response.data[0];
            this.type1Goods=response.data[1];
            this.type2Goods=response.data[2];
            this.type3Goods=response.data[3];
       })
       .catch(error=>{
           console.log('您的访问数据不存在！')
       })
   },
   //挂载 是指所有dom都加载完成时
   mounted:function(){
       var orderHeight=document.body.clientHeight;
    //    console.log(orderHeight);
    document.getElementById('order-list').style.height=orderHeight+'px'

   },
   methods:{
       addOrderList(goods){
           //总数量和总金额
           this.totalMoney=0;
           this.totalCount=0;
           //判断里面是否有这条数据
           let ishave=false;
           for(let i=0;i<this.tableData.length;i++){
               if(this.tableData[i].goodsId==goods.goodsId){
                   ishave=true;
               }
           }
           //根据判断的值编写业务逻辑
           if(ishave){
               //如果这条数据存在，则要更改数量
               //tableData里的数据进行过滤，留下ID相同的那条数据，filter返回的是新数组
               let arr =this.tableData.filter(o=>o.goodsId==goods.goodsId);
               arr[0].count++;

           }else{
               //如果这条数据不存在
               //声明一个新数组，将这条数据添加进去
               let newGoods={goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
               this.tableData.push(newGoods)

           }
           this.getAllMoney()
       },
       delSingleGodds(goods){
          if(this.tableData){
              this.tableData=this.tableData.filter(o=>o.goodsId !=goods.goodsId);
              this.getAllMoney()
          }
       },
       delAllGoods(){
         this.tableData=[];
         this.totalMoney=0;
         this.totalCount=0;
       },
       checkout(){
           console.log(this.totalCount);
         if(this.totalCount!=0){
           this.tableData=[];
           this.totalMoney=0;
           this.totalCount=0;
           this.$message({
               message:'结账成功！',
               type:'success'
           }); 
               
         }else{
             this.$message({
                 message:'结账失败',
                 type:'error'
             });
         }
       },
       getAllMoney(){
           if(this.tableData){
               this.totalMoney=0;
               this.totalCount=0;
               this.tableData.forEach((element)=>{
                    this.totalCount+=element.count;
                    this.totalMoney +=element.price*element.count;
                })
           }
       }    
   }

}
</script>


<style scoped>
.order-list{
    border-right: 1px solid #ccc;
    float: left
}
.goods-list{
    float: right;
}
.div-btn{
    margin-top: 10px;
}
.often-goods{
    padding: 10px;
}
.often-goods .title{
    text-align: left;
    border-bottom: 1px solid #ccc
}
.often-goods-list li{
    list-style: none;
    float: left;
    height:40px;
    line-height: 40px;
    background-color: #fff;
    font-size: 16px;
    margin-left: 40px;
    margin-top:10px
}
.often-goods-list .price{
   color:blue
}
.type-goods{
    clear: both;  
    padding: 10px 
}
.cookList li{
       list-style: none;
       width:23%;
       border:1px solid #E5E9F2;
       height: auot;
       overflow: hidden;
       background-color:#fff;
       padding: 2px;
       float:left;
       margin: 2px;
   }
   .cookList li span{
        display: block;
        float:left;
   }
   .foodImg{
       width: 40%;
   }
   .foodName{
       font-size: 18px;
       padding-left: 10px;
       color:brown;
   }
   .foodPrice{
       font-size: 16px;
       padding-left: 10px;
       padding-top:10px;
   }
   li{
       list-style:none;
       float: left;
   }
   .collect{
       margin-top: 20px
   }

</style>


<template>
    <div class='pos'>
        <el-row>
            <el-col :span=7 class='pos-order' id='list-order'> 
                <el-tabs >
                    <el-tab-pane label="点餐" >
                        <el-table style="width: 100%" :data="tableData" border>
                            <el-table-column prop='goodsName' label='商品名称'></el-table-column>
                            <el-table-column prop='price' label='价格' width='50'></el-table-column>
                            <el-table-column prop='count' label='数量' width='50'></el-table-column>
                            <el-table-column label='操作' width='100' fixed='right'> 
                                <template slot-scope="scope">
                                    <el-button type="text" size="small" @click='addGoods(scope.row)'>增加</el-button>
                                    <el-button type="text" size="small" @click='delSingleGoods(scope.row)'>删除</el-button>
                                </template>                        
                                    
                            </el-table-column>               
                        </el-table>
                    </el-tab-pane>
                    <div class='total'>
                        <span>数量：{{totalNum}}</span>
                        <span>总计：{{totalMoney}}</span>
                    </div>
                    <el-tab-pane label="挂单">
                        挂单
                    </el-tab-pane>
                    <el-tab-pane label="外卖">
                        外卖
                    </el-tab-pane>
                    <div style="margin-top:20px">
                         <el-button type="info" size="small" plain>挂单</el-button>
                        <el-button type="info" size="small" plain @click="delAllGoods">删除</el-button>
                        <el-button type="warning" size="small" plain @click="checkout">结账</el-button>
                    </div>
                </el-tabs>
            </el-col>
            <el-col :span=17 class='oftenGoods'>
                <div class='goodsTitle'>常用商品</div>
                <div class='goodsList'>
                    <ul>
                        <li v-for='goods in oftenGoods' :key='goods.goodsId' @click='addGoods(goods)'>
                            <span>{{goods.goodsName}}</span>
                            <span class='goodsPrice'>￥{{goods.price}}</span>
                        </li>
                    </ul>
                </div>

                <div class='classList'>
                    <el-tabs>
                        <el-tab-pane label='汉堡'>
                            <ul class='foodList'>
                                <li v-for='foods in type0Goods' :key='foods.goodsId' @click='addGoods(foods)'>
                                    <span class='foodImg'><img :src='foods.goodsImg' width='100%'/></span>
                                    <span class='foodName'>{{foods.goodsName}}</span>
                                    <span class='foodPrice'>${{foods.price}}</span>
                                </li>
                                <!--<li>
                                    <span class='foodImg'><img src='http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg' width="100%"/></span>
                                    <span class='foodName'>goodsName</span>
                                    <span class='foodPrice'>$price</span>
                                </li>-->
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane label='小食'>
                            <ul class='foodList'>
                                <li v-for='foods in type1Goods' :key='foods.goodsId' @click='addGoods(foods)'>
                                    <span class='foodImg'><img :src='foods.goodsImg' width='100%'/></span>
                                    <span class='foodName'>{{foods.goodsName}}</span>
                                    <span class='foodPrice'>${{foods.price}}</span>
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane label='饮品'>
                            <ul class='foodList'>
                                <li v-for='foods in type2Goods' :key='foods.goodsId' @click='addGoods(foods)'>
                                    <span class='foodImg'><img :src='foods.goodsImg' width='100%'/></span>
                                    <span class='foodName'>{{foods.goodsName}}</span>
                                    <span class='foodPrice'>${{foods.price}}</span>
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane label='套餐'>
                            <ul class='foodList'>
                                <li v-for='foods in type3Goods' :key='foods.goodsId' @click='addGoods(foods)'>
                                    <span class='foodImg'><img :src='foods.goodsImg' width='100%'/></span>
                                    <span class='foodName'>{{foods.goodsName}}</span>
                                    <span class='foodPrice'>${{foods.price}}</span>
                                </li>
                            </ul>
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
    created:function(){
        axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods')
        .then(response=>{
            //console.log(response)
            this.oftenGoods = response.data;
        })
        .catch(error=>{
            alert('网络错误！')
        })
        axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods')
        .then(response=>{
            //console.log(response);
            this.type0Goods = response.data[0];
            this.type1Goods = response.data[1];
            this.type2Goods = response.data[2];
            this.type3Goods = response.data[3];
        })
    },
    mounted:function(){
        var orderHeight = document.body.clientHeight;
        document.getElementById('list-order').style.height = orderHeight + 'px';
    },
    data(){
       return{
            tableData:[],
            oftenGoods:[],
            type0Goods:[],
            type1Goods:[],
            type2Goods:[],
            type3Goods:[],
            totalMoney:0,
            totalNum:0
        }
    },
    methods:{
        //添加订单列表的方法
        addGoods(goods){


            let isHave = false;
            //判断是否这个商品已经存在于订单列表
            for(let i=0;i<this.tableData.length;i++){
                if(goods.goodsId == this.tableData[i].goodsId){
                    isHave = true;
                }
            }
            //根据isHave的值判断订单列表中是否已经有此商品
            if(isHave){
                //存在就进行数量添加
                let arr = this.tableData.filter(o=>o.goodsId == goods.goodsId);
                //console.log(arr[0].count);
                arr[0].count++;
            //不存在就推入数组
            }else{
                let newGoods = {goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
                console.log(newGoods);
                this.tableData.push(newGoods);
            }
            this.totalAll();
        },
        delSingleGoods(goods){
            if(goods.count!=1){
                goods.count--;
            }else{
                this.tableData = this.tableData.filter(o=>o.goodsId != goods.goodsId);  
            }
            
            this.totalAll();
        },
        //进行数量和价格的汇总计算
        totalAll(){
            this.totalMoney = 0;
            this.totalNum = 0;
            
            this.tableData.forEach((element)=>{
                this.totalNum += element.count;
                this.totalMoney = this.totalMoney + (element.count * element.price);
            })
        },
        delAllGoods(){
            this.totalMoney = 0;
            this.totalNum = 0;
            this.tableData=[];
        },
        //结账
        checkout(){
           if(this.totalNum!=0){
                this.totalMoney = 0;
                this.totalNum = 0;
                this.tableData=[];
                this.$message({
                    message:'结账成功！',
                    type:'success'
                });
           }else{
               this.$message.error('请勿空结账！谢谢');
           } 
        }
    }
}
</script>
<style scoped>
.pos-order{
    border-right:1px solid #4D647E;

}
.total{
    border-bottom: 1px solid rgb(227, 229, 231);
    background-color: #FFF;
    padding: 10px
}
.total span{
    margin:0 10px;
}
.goodsTitle{
    padding: 10px;
    background-color: #4D647E;
    border-bottom: 1px solid rgb(227, 229, 231);
    text-align: left;
    color:#F7E5DD
}
.goodsList ul li{
    list-style: none;
    float: left;
    padding: 10px;
    background-color:#F7E5DD;
    box-shadow: 1px 1px 1px 1px  rgb(228, 230, 231);
    border: 1px solid rgb(228, 230, 231);
    margin: 10px;
    width:140px;
    height: 20px;
}
.goodsList ul li:hover{
    background-color:rgb(240, 184, 160);
    color:#FFF;
    cursor: pointer;
}
.goodsList ul li:hover .goodsPrice{
    color:rgb(92, 97, 102);
}
.goodsPrice{
    color:rgb(241, 130, 65);
}
.classList{
    clear: both;
    padding-top: 50px;
}
.foodList li{
    float: left;
    list-style: none;
    padding: 3px;
    margin: 10px;
    height: 60px;
    width:180px;
    border: 1px solid rgb(228, 230, 231);
    background-color:#F7E5DD;
    box-shadow: 1px 1px 1px 1px  rgb(228, 230, 231);
    font-size:14px;
    position: relative;
}
.foodList li span{
    display: block;
}

.foodImg{
    height:60px;
    width:60px;
    position: absolute;
    left: 3px;
}

.foodName{
    color:rgb(241, 130, 65);
    position: absolute;
    left:80px;
    top:5px;
}

.foodPrice{
position: absolute;
left:80px;
top:30px;
}
/*
.clearfix::after{
    content:'\0020';
    display: block;
    height: 0;
    clear: both;
}
.clearfix{
    zoom:1
}*/
</style>

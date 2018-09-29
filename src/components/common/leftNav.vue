<template>
    <div class='left-nav'>
        <ul>
            <li>
                <i class="icon iconfont icon-shouyin1"></i>
                <div>收银</div>
            </li>
            <li>
                <i class="icon iconfont icon-dianpu"></i>
                <div>店铺</div>
            </li>
            <li>
                <i class="icon iconfont icon-hanbao2"></i>
                <div>商品</div>
            </li>
            <li>
                <i class="icon iconfont icon-tubiao-"></i>
                <div>会员</div>
            </li>
            <li>
                <i class="icon iconfont icon-tongji"></i>
                <div>统计</div>
            </li>
            <li>
                <i class="icon iconfont icon-shezhi"></i>
                <div>设置</div>
            </li>
            <li>
                <i class="icon iconfont icon-gerenzhongxin"></i>
                <div>中心</div>
            </li>
        </ul>
    </div>
</template>
<script>
export default {
    name:'leftNav',
}
</script>
<style>
    .left-nav{
        float: left;
        height: 100%;
        background-color:#4D647E;
        width: 5%;
        color:#F7E5DD
    }
    .left-nav ul{
        padding:0;
        margin:0;
    }
    .left-nav li{
        list-style: none;
        padding: 10px;
        margin-bottom:10px; 
        border-bottom: 0.5px solid #e9e4d4
    }
    .iconfont{
        font-size:24px;
    }
</style>


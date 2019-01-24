<template>
    <div class="app-goodsinfo">
        商品详情 <!--轮播图  向子组件传参-->
        <swiper-box :list="list"></swiper-box> 
        <!--商品信息-->
        <div class="mui-card">
            <div class="mui-card-header">
             {{info.name}}
            </div>
            <div class="mui-card-content">
             <div class="mui-card-inner">
               <div class="price">
                市场价：<del>{{info.price}}</del>
                销售价：<span class="now">{{info.price*0.5}}</span>
               </div>
             购买数量
                <div class="mui-numbox" data-numbox-min='1' data-numbox-max='9'>
					<button class="mui-btn mui-btn-numbox-minus" type="button" @click="goodsSub">-</button>
					<input id="test" class="mui-input-numbox" type="number" value="1" v-model="val"/>
					<button class="mui-btn mui-btn-numbox-plus" type="button" @click="goodsAdd">+</button>
				</div>
            </div>

            <div class="mui-card-footer">
            <mt-button type="primary" size="small">立即购买</mt-button>
            <mt-button type="primary" size="small" @click="addCart">加入购物车</mt-button>
            </div>
            </div>
        </div>
    </div>
</template>

<script>
 import swiper from '../sub/swiper.vue'
 import {Toast} from 'mint-ui'
export default {
    created() {
        // console.log("list组件参数:"+this.id);
        this.getImageList();//轮播图
        this.getGoodsInfo();//商品详细信息
    },
    data() {
        return {
            id:this.$route.params.id,//接收参数
            list:[],
            val:1,
            info:{}
        }
    },
    components:{
        "swiper-box":swiper
    },
    methods: {
        getGoodsInfo(){
            //1:获取参数id
            var  id=this.id;
            //2:发送ajax请求获取商品信息
            var url="http://127.0.0.1:3000/getProduct?id="+id;
            this.axios.get(url).then(result=>{
                // console.log(result.data);
                this.info=result.data.data;
            })
            //3：保存显示模板
        },
        addCart(){
            //1:获取参数  uid pid count price 
            var uid=1;
            var pid=this.id;
            var count=this.val;
            var price=123;
            //2:发送ajax请求
            var url="http://127.0.0.1:3000/addCart?uid="+uid+"&pid="+pid+"&count="+count+"&price="+price;
            this.axios.get(url).then(result=>{
                if(result.data.code>0){
                    //如果添加成功就修改数量
                    this.$store.commit("increment")
                    Toast(result.data.msg)
                }else{
                     Toast(result.data.msg)
                }
            })
            //3：返回结果
            //4：显示提示消息

        },
        goodsSub(){
            if(this.val>1){this.val--;}
            
        },
        goodsAdd(){
            if(this.val<=998){this.val++;}
            
        },
        getImageList(){
            var url="http://127.0.0.1:3000/getImages";
            this.axios.get(url).then(result=>{
                this.list=result.data;
            })
        }
    },
}
</script>

<style>
    
</style>
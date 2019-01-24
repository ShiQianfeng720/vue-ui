<template>
   <div class="app-cart">
			<div class="mui-card">
				<div class="mui-card-header">
           <h3>购物车列表</h3>
        </div>
				<div class="mui-card-content">
					<div class="mui-card-content-inner">
             <!--商品项目 start-->
            <ul class="mui-table-view">
				<li class="mui-table-view-cell mui-media" v-for="item in list" :key="item.id">
					<a href="javascript:;">
						<img class="mui-media-object mui-pull-left" src="">
						<div class="mui-media-body">
                            <div class="fl">
							 {{item.name}}
							<p class='mui-ellipsis'> {{item.price}}  </p>
                            </div>
                                <!--购买数量-->
                        <div class="mui-numbox" data-numbox-min='1' data-numbox-max='9'>
                            <button class="mui-btn mui-btn-numbox-minus" type="button" @click="cartSub" :data-id="item.id">-</button>
                            <input id="test" class="mui-input-numbox" type="number" :value="item.count" />
                            <button class="mui-btn mui-btn-numbox-plus" type="button" @click="cartAdd" :data-id="item.id">+</button>
                        </div>
                            <!--购买数量end-->
                           
              
						</div>
                        
					</a>
				</li>
            </ul>
             <!--商品项目 end-->
					</div>
				</div>
				<div class="mui-card-footer">小计：{{getSubTotal}}</div>
        </div>
   </div>  
</template>
<script>
//1:创建元素
//2：发送请求获取购物车里的数据

  export default {
    data(){
      return {
          val:1,
          total:0,
          list:[]
      }
    },
    created() {
        this.getList()
    },
    methods: {
        getList(){
            var url="http://127.0.0.1:3000/getCartList";
            this.axios.get(url).then(result=>{
                this.list=result.data.data;
            })
        },
         cartSub(e){
            var id=e.target.dataset.id;
            //创建循环购物车数组内容
            for(var item of this.list){
                //判断当前元素id是否与元素id相同
                if(item.id==id && item.count>1){//修改数据
                    item.count--;
                    //同步数据
                    this.updateItemCount(item.id,item.count);
                    break;
                }
            }
        },
        cartAdd(e){
            //获取当前按钮绑定购物车id
            var id=e.target.dataset.id;
            //创建循环购物车数组内容
            for(var item of this.list){
                //判断当前元素id是否与元素id相同
                if(item.id==id){//修改数据
                    item.count++;
                    this.updateItemCount(item.id,item.count);
                    break;
                }
            }
        },
        updateItemCount(id,count){
            //购物车数量同步操作
                var url="http://127.0.0.1:3000/updateCart?id="+id+"&count="+count;
                this.axios.get(url).then(result=>{
                    console.log(result);
                    // this.count=result.count;
                })
        }
        
    },
    computed: {
        //3：小计 item.price*item.count
            getSubTotal:function(){
                var sum=0;
                for(var item of this.list){
                    sum+=item.price*item.count;
                }
                return sum;
            }

    },
  }  
</script>
<style> 
.fl{
    float:left;
}
    .mui-numbox{
        float:right;
    }
</style>
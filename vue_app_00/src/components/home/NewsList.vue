<template>
<div class="app-news">
    <ul class="mui-table-view">
				<li v-for="item of list" class="mui-table-view-cell mui-media" :key="item.id" >
                 <!--日期过滤器-->
					<router-link :to="'/NewsInfo?id='+item.id">
						<img  class="mui-media-object mui-pull-left" :src="item.img_url">
						<div class="mui-media-body">
							{{item.title}}
							<p class='mui-ellipsis'>
                            <span>{{item.ctime | datetimeFilter}}</span> <span>点击:{{item.point}}</span>  
                            </p>
						</div>
					</router-link>
				</li>
			</ul>
    <mt-button size="large" type="primary" @click="getMore">加载更多</mt-button>
</div>
</template>

<script>
export default {
    data(){
        return{
            list:[],//返回数据
            pageIndex:0, //当前页码
            pageSize:7, 
            pageCount:1 //总页数
        }
    },
    created() {
        this.getMore();
    },
    methods: {
        getMore(){
            //加载下一页
           this.pageIndex++;
           var pno=this.pageIndex;
           var ps=this.pageSize;
           //2:发送ajax请求
           var url="http://127.0.0.1:3000";
           url+="/getNews";
           url+="?pno="+pno+"&pageSize="+ps;
           //返回数据保存list
           this.axios.get(url).then(result=>{
               //this.list=result.data.data;
               //追加
               var rows=this.list.concat(result.data.data);
               this.list=rows;
               this.pageCount=result.data.pageCount;
           })

        }
        
    },
}
</script>

<style>
    .app-news .mui-ellipsis{
        display:flex;
        font-size:12px;
        color:#226aff;
        justify-content:space-between; /*两端对齐 */
    }
</style>
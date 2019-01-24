<template>
    <div class="app-newsinfo">
        <h3>{{info.title}}</h3>
        <h5>{{info.ctime|datetimeFilter}}</h5>
        <p>{{info.content}}</p>
        <!--3：调用评论的子组件-->
        <comment-box :id="id"></comment-box>
    </div>
</template>

<script>
//1:引入评论的子组件
import comment from '../sub/comment.vue';
export default {
    data() {
        return {
            info:{},
            id:this.$route.query.id
        }
    },
    //2：注册子组件
    components:{
        "comment-box":comment,
       
    },
    created() {
        //this.$route.query.id;
        this.getNewsInfo();
    },
    methods: {
        getNewsInfo(){
            //1:获取参数id
            var id=this.$route.query.id
            //2:创建变量保存url地址
            var url="http://127.0.0.1:3000";
            url+="/getNewsInfo?id="+id
            //3:发送ajax请求数据获取
            this.axios.get(url).then(result=>{
                // console.log(result);
                this.info=result.data.data;
            })
            //将返回的结果保存到info对象
        }
    },
}
</script>

<style>
    .app-newsinfo{
        background-color:#fff;
        border:1px solid #aaa;
    }
    .app-newsinfo h3{
        font-size:18px;
    }
</style>
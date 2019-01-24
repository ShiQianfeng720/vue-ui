<template>
    <div>
       <div class="mui-card">
				<div class="mui-card-header"> 
                <h3>用户登陆</h3>
                </div>
				<div class="mui-card-content">
					<div class="mui-card-content-inner">
						<form>
                        用户名：
                        <input type="text" v-model="name" placeholder="请输入用户名" />
                        密码：
                        <input type="password" v-model="pwd" placeholder="请输入密码"/>
                        
                        <mt-button size="large" @click.prevent="handLogin">登陆</mt-button>
                        </form>
					</div>
				</div>
				<div class="mui-card-footer"></div>
                
			</div>
    </div>

</template>

<script>
 import {Toast} from 'mint-ui'
export default {
   
    data() {
        return {
            name:"",
            pwd:""
        }
    },
    methods: {
        handLogin(){
            //1:获取用户输入的用户名和密码
            var n=this.name;
            var p=this.pwd;
            //2：验证
            var reg=/^[a-z0-9_]{3,12}$/i;
            if(!reg.test(n)){
                Toast("用户名格式不正确");
                return;
            }//3：验证失败发消息
            if(!reg.test(p)){
                Toast("密码名格式不正确");
                return;
            }
            // console.log(n+":"+p);
            //4：发送ajax请求
            var url="http://127.0.0.1:3000/login?name="+n+"&pwd="+p;
            this.axios.get(url).then(result=>{
                if(result.data.code>0){
                     this.$router.push("/Home")
                }else{
                     Toast(result.data.msg)
                }
            })
            //5：成功就跳转到home页面
            //6：失败提示消息
        }
    },
}
</script>
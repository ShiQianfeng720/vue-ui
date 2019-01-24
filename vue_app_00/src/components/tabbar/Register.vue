<template>
    <div class="app-register">
   
       	<div class="mui-card">
				<div class="mui-card-header"> 
                <h3>用户注册</h3>
                </div>
				<div class="mui-card-content">
					<div class="mui-card-content-inner">
						<form>
                        用户名：
                        <input type="text" v-model="name" placeholder="请输入用户名" @blur.prevent="checkName"/>
                        密码：
                        <input type="password"  v-model="pwd1" placeholder="请输入密码"/>
                        确认密码：<input type="password" v-model="pwd2" placeholder="请确认密码"/>
                        <mt-button size="large" @click="register">注册</mt-button>
                        </form>
					</div>
				</div>
				<div class="mui-card-footer"></div>
                
			</div>
    </div>
</template>

<script>
import { Toast } from "mint-ui";
export default {
  data() {
    return {
      name: "", //双向绑定数据
      pwd1: "",
      pwd2: "",
      isSubmit: false //是否允许提交
    };
  },
  created() {},
  methods: {
    checkName() {
      //1：获取用户输入的用户名
      var name = this.name;
      //2：发送ajax请求
      var url = "http://127.0.0.1:3000/existsname?name=" + name;
      this.axios.get(url).then(result => {
        if (result.data.code > 0) {
          //3：提醒消息
          alert(result.data.msg);
          this.isSubmit = true;
        } else {
          alert(result.data.msg);
          this.isSubmit = false;
        }
      });
    },
    register() {
      //1:获取用户名 密码 确认密码
      // if (this.isSubmit === false) {
      //   //禁止提交
      //   return;
      // }
      var name = this.name;
      var pwd1 = this.pwd1;
      var pwd2 = this.pwd2;
      // console.log(name+"|"+pwd1+"|"+pwd2);
      //2:验证用户名和密码是否正确
      var regname = /^[a-z0-9_]{6,12}$/i;
      var regpwd = /^[a-z0-9]{8,12}$/i;
      if (!regname.test(name)) {
        Toast("用户名格式不正确");
        return;
      }
      if (!regpwd.test(pwd1)) {
        Toast("密码格式不正确");
        return;
      }
      if (pwd1 != pwd2) {
        Toast("两次密码不一致，请修改");
        return;
      }
      
      if (pwd1 == pwd2) {
        //如果正确发送ajax请求
        var url =
          "http://127.0.0.1:3000/register?name=" + name + "&pwd=" + pwd1;
        this.axios.get(url).then(result => {
          if (result.data.code > 0) {
            Toast(result.data.msg);
          } else {
            Toast(result.data.msg);
          }
        });
      }
    }
  }
};
</script>
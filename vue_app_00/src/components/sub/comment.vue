<!--src/compoents/sub/comment.vue-->
<template>
    <div class="app-comment">
        <h3>评论子组件</h3>
        <!--发表评论-->
        <textarea placeholder="吐槽大会开始啦...." maxlength="120" v-model="msg"></textarea>
       <mt-button size="large" @click="addComment">发表评论</mt-button>
       <!--显示评论区域-->
       <div class="cmt-list">
        <div class="cmt-item" v-for="(item,i) in list" :key="item.id">
        第{{i+1}}楼：发表时间{{item.ctime|datetimeFilter}}
            <div class="cmt-body">
                {{item.content}}
            </div>
        </div>
       </div>
       <mt-button size="large" @click="getMore">更多评论</mt-button>
    </div>
</template>
<script>
import { Toast } from "mint-ui";
export default {
  data() {
    return {
      msg: "",
      pageIndex: 0, //当前页码
      pageSize: 7, //页大小
      pageCount: 1, //总页数
      list: [] //当前页内容
    };
  },
  props: ["id"],
  created() {
    console.log(this.id);
    this.getMore();
  },
  methods: {
    getMore() {
      var nid = this.id; //获得新闻编号 nid pno pagesize
      this.pageIndex++; //当前页码自增
      var pno = this.pageIndex;
      var ps = this.pageSize;
      //创建变量url
      var url =
        "http://127.0.0.1:3000/getComments?pno=" +
        pno +
        "&nid=" +
        nid +
        "&pageSize=" +
        ps;
      //发送ajax请求
      this.axios.get(url).then(result => {
        console.log(result);
        //获取返回数据 追加
        var rows = this.list.concat(result.data.data);
        this.list = rows;
      });
    },
    addComment() {
      //获得两个参数
      var nid = this.id; //新闻编号
      var content = this.msg; //获取评论的内容
      var size = content.trim().length; //内容长度
      if (size == 0) {
        //如果长度为0，显示评论内容不能为空
        Toast("评论不能为空");
        return;
      }
      //发送ajax请求
      var url =
        "http://127.0.0.1:3000/addComment?nid=" + nid + "&content=" + content;
      this.axios.get(url).then(result => {
        if (result.data.code == 1) {
          //接受返回数据
          Toast(result.data.msg); //toast()提示成功
          this.msg = "";
          //获取最新评论内容
          this.list = []; //清空原有数据
          this.pageIndex = 0; //清空页码
          this.getMore();
        } else {
          Toast(result.data.msg);
        }
      });
    }
  }
};
</script>

<style>
.app-comment h3 {
  font-size: 18px;
}
.app-comment textarea {
  font-size: 14px;
  height: 100px;
  margin: 0;
}
.app-comment .cmt-list {
  margin: 5px 0;
}
.app-comment .cmt-list .cmt-item {
  border: 1px solid #aaa;
  margin-top: 15px;
}
</style>
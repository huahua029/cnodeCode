<template>
  <div class="wrapper">
    <!-- 在数据没返回的时候，显示这个正在加载的图片 -->
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif">
    </div>
    <!-- 数据返回，列表 -->
    <div class="posts">
      <ul>
        <!-- 内容顶部 -->
        <li>
          <div class="topbar">
            <span>全部</span>
            <span>精华</span>
            <span>分享</span>
            <span>问答</span>
            <span>招聘</span>
          </div>
        </li>
        <li v-for="post in posts">
          <!-- 头像 -->
          <img :src="post.author.avatar_url">
          <!-- 回复/浏览 -->
          <span>
            <span class="count">{{post.reply_count}}</span>/
            <span class="count">{{post.visit_count}}</span>
          </span>
          <!-- 分类 -->
          <span :class="[
            {put_good: (post.good==true)},
            {put_top: (post.top==true)},
            {topiclist_tab:(post.good != true && post.top != true)}
            ]">
            <span>{{post | tabFormate}}</span>
          </span>
          <!-- 标题 -->
          <router-link :to="{name:'post_content',params:{
            id: post.id,
            name: post.author.loginname
          }}">
            <span class="title">
              {{post.title}}
            </span>  
          </router-link>
          <!-- 时间 -->
          <span class="last_reply">{{post.last_reply_at | formatDate}}</span>
        </li>
        <pagination @handleList='renderList'></pagination>
      </ul>
    </div>
  </div>
</template>

<script>
import pagination from './Pagination'
export default {
  name: 'PostList',
  data(){
    return {
      isLoading: false,
      posts:[], //代表页面的列表数组
      postpage: 1
    }
  },
  components: {
    pagination
  },
  methods: {
    getData(){
      this.$http.get('https://cnodejs.org/api/v1/topics',{
        params:{
          page:this.postpage,
          limit:20
        }
      })
      .then(res=>{
        this.isLoading = false  //加载成功去掉动画
        this.posts = res.data.data
      })
      .catch(function(err){
        console.log(err) //处理返回失败后的问题
      })
    },
    renderList(value){
      this.postpage = value
      this.getData()
    }
  },
  created(){
      this.isLoading = true  //加载成功之前显示加载动画
      this.getData()  //在页面加载之前获取数据
    }
}
</script>

<style scoped>
*{margin: 0;padding: 0;vertical-align: middle;}
img{
  width: 30px;
}
.put_top,
.put_good{
  font-size: 14px;
  background-color:#80bd01;
  border-radius: 2px;
}
.topiclist_tab{
  font-size: 14px;  
  border-radius: 2px;
  background: #e5e5e5;
}
ul{
  list-style: none;
}
li{
  border-bottom: 1px solid #e1e1e1;
  line-height: 16px;
  padding:10px 10px;
}
li>span{
  display: inline-block;
}
.topbar{
  font-size: 14px;
  background: #f6f6f6;
  color: #80bd01;
  padding-right: 5px;
}
.topbar>span{
  display: inline-block;
  padding: 5px 15px;
}
.topbar>span:hover{
  color: blue;
}
.count{
  color:#333;
  font-size: 14px;
}
.last_reply{
  display: inline-block;
  float: right;
}
</style>

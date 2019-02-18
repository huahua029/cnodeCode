<template>
  <div class="article">
    <!-- 如果正在加载显示这个 -->
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif">
    </div>
    <div v-else>
      <div class="topic_header">
        <div class="topic_title">{{post.title}}</div>
        <ul>
          <li>发布于{{post.create_at | formatDate}}</li>
          <li>作者{{post.author.loginname}}</li>
          <li>{{post.visit_count}}次浏览</li>
          <li>来自{{post | tabFormate}}</li>
        </ul>
        <div v-html='post.content' class="topic_content"></div>
      </div>
      <div>
        <!-- 回复区 -->
        <div class="topbar" >回复</div>
        <div v-for="(reply,index) in post.replies">
          <router-link :to="{
            name: 'user_info',
            params:{
              name: reply.author.loginname
            }
          }">
            <img :src="reply.author.avatar_url"> 
          </router-link>
          
          <span>{{index+1}}楼</span>
          <span v-if="reply.ups.length>0">
            {{reply.ups.length}}
          </span>
          <p v-html="reply.content "></p>
        </div>
      </div>
    </div>
  </div>  
</template>

<script>
export default {
  name: 'Article',
  data(){
    return {
      isLoading: false,
      post: {} //代表当前文章页的所有内容
    }
  },
  methods:{
    getArticleDate(){
      this.$http.get(`https://cnodejs.org/api/v1/topic/${this.$route.params.id}`)
        .then((res)=>{
          if(res.data.success == true){
            this.isLoading = false
            this.post = res.data.data
          }
        })
        .catch(function(err){
          console.log(err)
        })
    }
  },
  beforeMount(){
    this.isLoading = true
    this.getArticleDate()
  },
  // 检测路由变化
  watch:{
    '$route'(to,from){
      this.getArticleDate()
    }
  }
}
</script>

<style>
@import url('../assets/markdown-github.css');
</style>


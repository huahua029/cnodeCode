<template>
  <div class="UserInfo">
    <!-- 如果正在加载显示这个 -->
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif">
    </div>
    <!-- 内容 -->
    <div class="userInfomation">
      <section>
        <img :src="userinfo.avatar_url">
        <span>{{userinfo.loginname}}</span>
        <p>{{userinfo.score}}积分</p>
        <p>注册时间{{userinfo.create_at | formatDate}}</p>
      </section>
      <!-- 回复的主题 -->
      <div class="replies">
        <ul>
          <li v-for="item in userinfo.recent_replies">
            <router-link :to="{
              name: 'post_content',
              params: {
                id: item.id
              }
            }">
              {{item.title}}
            </router-link>
          </li>
        </ul>
      </div>
        <!-- 创建的主题 -->
      <div class="topics">
        <ul>
          <li v-for="item in userinfo.recent_topics">
            <router-link :to="{
              name: 'post_content',
              params: {
                id: item.id
              }
            }">
              {{item.title}}
            </router-link>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'UserInfo',
  data(){
    return {
      isLoading: false,
      userinfo:{}
    }
  },
  methods: {
    getData(){
      this.$http.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
        .then((res)=>{
            this.isLoading = false
            this.userinfo = res.data.data
        })
        .catch(function(err){
          console.log(err)
        })
    }
  },
  beforeMount(){
      this.isLoading = true  //加载成功之前显示加载动画
      this.getData()  //在页面加载之前获取数据
    }
}
</script>
<style scoped>

</style>



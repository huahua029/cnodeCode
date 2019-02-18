<template>
  <div class="pagination">
    <!-- 不带参数，传的原生对象 -->
    <button @click='changeBtn'>首页</button>
    <button @click='changeBtn'>上一页</button>
    <button v-if="jduge">...</button>    
    <button v-for="btn in pagebtn"
    @click='changeBtn(btn)'
    :class="[{currentPage:btn==currentPage},'pagebtn']"
    >
      {{btn}}
    </button>
    <button @click='changeBtn'>下一页</button>
  </div>
</template>

<script>
import $ from 'jquery'

export default {
  name: 'Pagination',
  data(){
    return {
      pagebtn: [1,2,3,4,5,'...'],
      currentPage: 1,
      jduge: false
    }
  },
  methods: {
    changeBtn(page){
      //点击上一页，下一页
      if(typeof page != 'number'){
        switch(page.target.innerText){
          case '上一页': 
            $('button.currentPage').prev().click()
            break
          case '下一页':
            $('button.currentPage').next().click() 
            break      
          case '首页':
            this.pagebtn = [1,2,3,4,5,'...'] 
            this.changeBtn(1)
            break
          default: 
            break          
        }
        return 
      }
      // 中间
      this.currentPage = page
      if(page>4){
        this.jduge = true
      }else{
        this.jduge = false
      }
      if(page == this.pagebtn[4]){ //移除第一个，添加最后一个
        this.pagebtn.shift() //移除第一个元素
        this.pagebtn.splice(4,0,this.pagebtn[3]+1)
      }else if(page == this.pagebtn[0] && page != 1){
        this.pagebtn.unshift(this.pagebtn[0]-1)
        this.pagebtn.splice(5,1)
      }
      this.$emit('handleList',this.currentPage)
    }
  }
}
</script>
<style scoped>
.pagebtn{
  background: grey;
}
.currentPage{
  background: red;
}
</style>


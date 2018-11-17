<template>
 <div class="article">
     <div class="article_content bg-default">
           <div class="loading" v-if="isLoading">
              <img src="../assets/loading.gif" alt="loading">
          </div>
          <div v-else>
              <div class="topic_header">
                  <div class="topic_title"><span>{{post | tabFormatter}}</span>{{post.title}}</div>
                  <ul>
                      <li>• 发布于：{{post.create_at |formatDate}}</li>
                      <li>• 作者：{{post.author.loginname}}</li>
                      <li>• {{post.visit_count}} 次浏览</li>
                      <li>• 来自{{post | tabFormatter}} </li>
                  </ul>
              </div>
             <div class="topic_content markdown-body" v-html="post.content"></div>
          </div>        
     </div>
     <div class="bg-default article_msg">

        <div class="topbar">评论</div>
        <div class="reply_msg" v-for="(reply,index) in post.replies" :key="index">
            <router-link :to="{
                name:'user_info',
                params:{
                    name:reply.author.loginname,
                }
            }">
                <img :src="reply.author.avatar_url" alt="">
            </router-link>
            <router-link :to="{
                name:'user_info',
                params:{
                    name:reply.author.loginname,
                }
            }">
                <span>{{reply.author.loginname}}</span>
            </router-link>
            <span>{{index+1}}楼</span>
            <!-- <span v-if="reply.ups.length>0">
                {{reply.ups.length}}赞
            </span> -->
            <span>
                <p v-html="reply.content"></p>
            </span>
        </div>
     </div>

 </div>
</template>

<script>
export default {
  name:'Article',
  data() {
     return {
         isLoading:false,
         post:{} 
 }
  },
 methods:{
     getArticleData(){
         this.$axios.get(`https://cnodejs.org/api/v1/topic/${this.$route.params.id}`)
         .then(res=>{      
             if(res.data.success == true){
                 this.isLoading = false,
                 this.post =res.data.data
             }
         })
         .catch(err=>{
         })
     }
 },
     beforeMount(){
        this.isLoading=true,
        this.getArticleData()
    },
    watch:{
        '$route'(to,from){
            this.getArticleData()
        }
    }
}
</script>


<style scoped>
.article{
    margin-right:310px;
}
.topic_header{
    padding:10px 0;
    border-bottom: 1px solid #e5e5e5;
}
.topic_title{
    padding:10px;
    font-size: 20px;
    font-weight: bold;

}
.topic_title>span{
    padding: 2px 4px;
    margin-right: 5px;
    font-size: 12px;
    color: #fff;
    border-radius: 3px;
    background: #80bd01;
}
.topic_header>ul{
    font-size: 11px;
    padding: 5px 10px;
}
.topic_header>ul>li{
    display: inline-block;
    color: #838383;
    margin-left:5px;
}
.topic_content{
    padding:10px 20px;;
}
.article_msg{
    margin-top:20px;
    border-radius:4px;
    overflow: hidden;
}
.article_msg img{
    width: 30px;
    height: 30px;
    border-radius: 3px;
}
.article_msg span{
    font-size: 12px;
}
.reply_msg{
    padding:10px 14px 10px 14px;
    border-bottom:1px solid #f0f0f0;
}
.reply_msg p{
    padding-left:32px;
    font-size: 16px;
}
@media(max-width:768px){
    .article{
    margin:auto;
}
}
</style>


<template>
  <div class="post-list">
      <div>
          <div class="loading" v-if="isLoading">
              <img src="../assets/loading.gif" alt="loading">
          </div>
          <div v-else>
            <ul>
                <li>
                        <span>文章列表</span>                                   
                </li>
                <li v-for="post in posts" :key="posts[post]">
                    <router-link :to="{
                        name:'user_info',
                        params:{
                            name:post.author.loginname,
                        }
                    }">
                        <img :src="post.author.avatar_url" alt="">
                    </router-link>

                    <span class="all_count">
                        <span class="reply_count">{{post.reply_count}}</span>/
                        <span class="visit_count">{{post.visit_count}}</span>                        
                    </span>

                    <span :class="{
                        'put-good':(post.good == true),
                        'put-top':(post.top == true),
                        'topiclist-tab':(post.good != true && post.top != true)}">
                        {{post | tabFormatter}}
                    </span>
                    <router-link class="article_title_a" :to="{
                        name:'post_content',
                        params:{
                            id:post.id,
                            name:post.author.loginname
                        }
                    }">
                        <span class="article_title">{{post.title}}</span>
                    </router-link>

                    <span class="last_reply">{{post.last_reply_at | formatDate}}</span>
                </li>

                <router-view name="listGood"></router-view>

                <li>
                    <pagination @handleList="renderList"></pagination>
                </li>
            </ul>
          </div>
      </div>
  </div>
</template>

<script>
import pagination from './Pagination'
export default {
  name:'Postlist',
  data() {
     return {
         isLoading:false,
         posts:[],
         postpage:1
    }
    },
    components:{
        pagination,
    },
    methods:{
        renderList(value){
            this.postpage = value
            this.getData()
        },
        getData(){
            this.$axios.get('https://cnodejs.org/api/v1/topics',{
                params:{
                    page:this.postpage,
                    limit:15
                }
                })
            .then((res)=>{
                this.isLoading = false
                this.posts = res.data.data
            })
            .catch((err)=>{
                console.log(err)
            })
        }
    },
    beforeMount(){
         this.isLoading = true
         this.getData()
    }
}
</script>

<style scoped>
.post-list{
        background-color: #fff;
        border-radius: 4px;
        overflow: hidden;
}
img{
    width:30px;
    height:30px;
    border-radius: 3px;
}
.reply_count{
    color: #9e78c0;
}
.visit_count{
    color: #b4b4b4;
}
.all_count{
    font-size: 10px;
    width:70px;
    text-align: center;
}
.put-good,.put-top{
    margin:0 10px;
    padding:3px 5px;
    border-radius:4px;
    font-size: 12px;
    color:#fff;
    background: #80bd01;
}
.topiclist-tab{
    margin:0 10px;
    padding:3px 5px;
    border-radius:4px;
    font-size: 12px;
    color:#999;
    background: #e5e5e5; 
}
.article_title{
    color: #333;
}
.last_reply{
    color: #778087; 
    font-size: 11px;
    position: absolute;
    right:10px;
}

ul{
   border-radius: 4px;
   position: relative;
}
ul>li{
    padding:10px;
    border-top: 1px solid #f0f0f0;
    display: flex;
    align-items: center;
    position: relative;
}
ul>li:hover{
    background: #f6f6f6;
}
ul>li:last-child:hover{
    background: transparent;
}
ul>li:first-child{
    background: #f6f6f6;
 
}
ul>li:first-child span{
    margin:4px 12px;
    padding:4px 8px;
    border-radius: 2px;
    color:#80bd01;
}
/* ul>li:first-child span:hover{
    background: #80bd01;
    color:#fff;
} */
@media(max-width:768px){
img{
    width:22px;
    height:22px;
}
ul>li{
    position: relative;
    padding: 20px 10px;
}
ul>li:first-child{
     padding:10px 0; 
}
.all_count{
    position: absolute;
    left: 80px;
    top: 45px;
}
.put-good,.put-top{
    font-size: 10px;
    min-width:25px;
    text-align: center;
}
.article_title{
    color: #333;
    font-size: 14px;
    width:100%;
    display: inline-block;
    white-space: nowrap;
    text-overflow:ellipsis;
    overflow: hidden;
}
.article_title_a{
    display: inline-block;
    max-width:65%;
    min-width:50%;
    overflow: hidden;
}

}

</style>

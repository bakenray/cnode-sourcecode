<template>
 <div class="slide_bar">
        <div class=" bg-default autherinfo">
            <div class="topbar">作者：</div>
            <div class="sidebar-info">
            <router-link :to="{
                name:'user_info',
                params:{
                    name:userinfo.loginname,
                }
            }">
                <img :src="userinfo.avatar_url" alt="">
            </router-link>

            <router-link :to="{
                name:'user_info',
                params:{
                    name:userinfo.loginname,
                }
            }">
                <span>{{userinfo.loginname}}</span>
            </router-link>

            </div>
        </div>
        <div class="bg-default recent_topics">
            <div class="topbar">作者最近主题：</div>
            <ul>
                <li v-for="list in topiclimitby5" :key="list.id">
                    <router-link :to="{
                        name:'post_content',
                        params:{
                            id:list.id,
                            name:list.author.loginname
                        }
                    }">
                        {{list.title}}
                    </router-link>
                </li>
            </ul>
        </div>
        <div class=" bg-default recent_replies">
            <div class="topbar">作者最近回复</div>
            <ul>
                <li v-for="list in replylimitby5" :key="list.id">
                    <router-link :to="{
                        name:'post_content',
                        params:{
                            id:list.id,
                            name:list.author.loginname
                        }
                    }">
                        {{list.title}}
                    </router-link>
                </li>
            </ul>
        </div>
 </div>
</template>

<script>
export default {
    name:'SlideBar',
    data() {
        return {
            userinfo:{},
            topiclimitby5:[],
            replylimitby5:[]
        }
    },
    methods:{
        getData(){
            this.$axios.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
            .then((res)=>{
                this.isLoding = false
                this.userinfo = res.data.data   
                this.topic_limit_by5()
                this.reply_limit_by5()
            })
            .catch((err)=>{
                console.log(err)
            })
        },
        topic_limit_by5(){
            if(this.userinfo.recent_topics.length>4){
                this.topiclimitby5 =  this.userinfo.recent_topics.slice(0,4)
            }else{
                 this.topiclimitby5 = this.userinfo.recent_topics
            }
        },
        reply_limit_by5(){
            if(this.userinfo.recent_replies.length>4){
               this.replylimitby5 = this.userinfo.recent_replies.slice(0,4)
            }else{
                this.replylimitby5 = this.userinfo.recent_replies
            }
        }        
    },
    beforeMount(){
        this.isLoding = true,
        this.getData()
    }
}
</script>

<style scoped>
.slide_bar{
    float:right;
    width:290px;
}
.recent_topics,.recent_replies{
    margin-top:20px;
}
.sidebar-info,.recent_topics ul,.recent_replies ul{
    padding:12px;
}
.sidebar-info img{
    width:48px;
    height:48px;
}
.sidebar-info span{
    font-size: 18px;
    vertical-align: top;
    color: #778087;
    padding-left:5px;
}
.slide_bar ul li{
    font-size: 14px;
    padding:5px 0;
    color: #778087;
    white-space:nowrap;
    text-overflow:ellipsis;
    overflow:hidden;
}
</style>

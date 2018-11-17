<template>
 <div class="user_info">
    <div class="loading" v-if="isLoading">
              <img src="../assets/loading.gif" alt="loading">
    </div>
    <div class="user_info_cont" v-else>
        <section class="bg-default info_box">
            <img :src="userinfo.avatar_url" alt="">
            <span>{{userinfo.loginname}}</span>
            <p>{{userinfo.score}}积分</p>
            <p>注册时间：{{userinfo.create_at |formatDate}}</p>
        </section>
        <div class="bg-default replies">
            <div class="topbar">最近参与的话题</div>
            <ul>
                <li v-for="(item,index) in userinfo.recent_replies" :key="index">
                    <router-link :to="{
                        name:'post_content',
                        params:{
                            id:item.id
                        }
                    }">
                        {{item.title}}
                    </router-link>
                </li>
            </ul>
        </div>
        <div class="bg-default topics">
            <div class="topbar">最近创建的话题</div>
            <ul>
                <li v-for="(item,index) in userinfo.recent_topics" :key="index">
                    <router-link :to="{
                        name:'post_content',
                        params:{
                            id:item.id
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
    name:'UserInfo',
    data() {
        return {
            isLoading:false,
            userinfo:{}
        }
    },
    methods:{
        getData(){
            this.$axios.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
            .then((res)=>{
                this.isLoading = false
                this.userinfo = res.data.data
            })
            .catch((err)=>{
                console.log(err)
            })
        }
    },
    beforeMount(){
        this.isLoading=true,
        this.getData()
    }
}
</script>

<style scoped>
.info_box img{
    width:40px;
    height:40px;
    border-radius:4px;
}
.info_box {
    padding:12px;
}
.replies>ul>li,.topics>ul>li {
    padding:10px;
    border-top: 1px solid #f0f0f0;
}
.topics,.replies{
    margin-top:20px;
}
</style>

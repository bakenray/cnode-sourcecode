<template>
 <div class="pagination">
     <button class="pagebtn btn-index" @click="changeBtn">首页</button>
     <button class="pagebtn" @click="changeBtn">上一页</button>
     <button class="pagebtn" v-if="jduge">...</button>
     <button v-for="btn in pagebtns" :class="[{currentPage:btn ==currentPage},'pagebtn']" :key="btn" @click="changeBtn(btn)">{{btn}}</button>
     <button class="pagebtn" @click="changeBtn">下一页</button>
 </div>
</template>

<script>
import $ from 'jquery'

export default {
    data() {
        return {
            pagebtns:[1,2,3,4,5,'...'],
            currentPage:1,
            jduge:false
        }
    },
    methods:{
        changeBtn(page){
            
            if(typeof page != 'number'){
                switch(page.target.innerText){
                    case '上一页':
                    $('button.currentPage').prev().click()
                    break
                     case '下一页':
                    $('button.currentPage').next().click()
                    break
                    case '首页':
                    this.pagebtns = [1,2,3,4,5,'...']
                    this.changeBtn(1)
                    break
                    default:
                    break
                }
                return
            }
            this.currentPage = page
            if(page>4){
                this.jduge=true
            }
            else{
                this.jduge=false
            }
            if(page===this.pagebtns[4]){
                this.pagebtns.shift()
                this.pagebtns.splice(4,0,this.pagebtns[3]+1)
            }
            else if(page===this.pagebtns[0] && page!==1){
                this.pagebtns.unshift(this.pagebtns[0]-1)
                this.pagebtns.splice(5,1)
            }
            this.$emit('handleList',this.currentPage)

        }
    }
}
</script>

<style scoped>

.pagebtn{
    padding: 8px 12px;
    border: 1px solid #e1e1e1;
    background: #fff;
    outline: none;
    margin: 0 2px;
    border-radius: 2px;
    color: #778087;
    cursor: pointer;
}
.pagebtn:hover{
    background: #80bd01;
    color: #fff;
    border-color:#6ea300;
}
.currentPage{
    background-color: #80bd01;
    color: #fff;
    border-color:#6ea300;
} 
.pagination{
    margin:auto;
}
@media(max-width:768px){
   .pagebtn{
    padding: 4px 8px;
    margin: 0 1px;
    font-size: 10px;
} 
.btn-index{
    display:none;
}
}
</style>

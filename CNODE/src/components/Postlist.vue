<template>
    <div>
        <div class="loading" v-if="isLoading">
             <img src="../assets/loading.gif" alt="">
        </div>
        
        <div class="post">
            <ul>
                <li class="toobar">
                  <div>
                    <span>全部</span>
                    <span>精华</span>
                    <span>分享</span>
                    <span>问答</span>
                    <span>招聘</span>
                  </div>
                </li>
                <li v-for="post in posts" :key="post">
                    <img :src="author.avatar_url" alt="">
                    <span>
                        <span class="reply_count">{{post.reply_count}}</span>
                        {{post.visit_count}}
                        <span :class="[{classify: true,'put-good':(post.good===true),'put-top':(post.top===true),
                        'other-tab': (post.good !== true && post.top !== true)
                        }]">
                            {{post | tabFormatter}}
                        </span>
                    </span>
                    <span>{{post.title}}</span>
                    <span class="last_reply">{{post.last_reply_at}}</span>

                </li>
            </ul>   
        </div>
        
    </div>
    
    
</template>


<script>
export default {
    data(){
        return{
            isLoading:false,
            posts:[]
        }
    },
    methods:{
        getData(){
            this.$http.get('https://cnodejs.org/api/v1/topics ',
            {
                page:1,  
                limit:20
            }
            ).then(res=>{
                this.isLoading=false,
                this.posts=res.data.data


            }).catch(err=>{

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

li{
    list-style: none;
}
.loading {
    width: 50px;
    height: 50px;
    margin: auto;
    position: absolute;
    top: 0;
    right: 0;
    left: 0;
    bottom: 0;
}
.post > ul {
  list-style: none;
  background-color: #fff;
}
.post > ul > li {
  padding: 10px;
  display: flex;
  align-items: center;
  white-space: nowrap;
}
.post > ul > li:first-child {
    background-color: #f6f6f6;
    color: #80bd01;
    font-size: 12px;
    
}
.post > ul > li:first-child  span {
    margin: 0 10px;
}
.post > ul > li:not(:last-child):hover {
    background-color: #f6f6f6;
}
.post > ul > li:not(:last-child) {
    border-bottom: 1px solid #f0f0f0;
}
.post > ul > li > a {
    font-size: 0;
}
.post > ul > li > a > img {
  width: 30px;
}
.reply-visit-count {
    float: left;
    width: 80px;
    text-align: center;
    display: inline-block;
    
}
.reply-count {
    font-size: 14px;
    color: #9e78c0;
    vertical-align: middle;
}
.count-slash {
    font-size: 10px;
    vertical-align: middle;
}
.visit-count {
    font-size: 10px;
    color: #b4b4b4;
    vertical-align: middle;
}
.post-title {
    max-width: 70%;
    font-size: 16px;
    color: #333;
    overflow: hidden;
    text-overflow: ellipsis;
}
.last-reply-at {
    
    margin-left: auto;
    font-size: 11px;
    color: #999;
    
}
.classify {
    float: left;
    font-size: 12px;
    margin: 0 5px;
    padding: 2px 4px;
    border-radius: 2px;
}
.put-good, .put-top {
    color: #fff;
    background-color: #80bd01;
}
.other-tab {
    color: #999;
    background-color: #e5e5e5;
}
</style>

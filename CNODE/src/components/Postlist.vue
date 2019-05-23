<template>
  <div class="PostList">
      <!-- 在数据未返回的时候，显示这个正在加载的gif -->
      <div class="loading" v-if="isLoading">
        <img src="../assets/loading.gif">
      </div>
      <!-- 代表主题列表 -->
      <div class="post" v-else>
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
          <li  v-for="post in posts" :key="post">
            <a href="#">
              <img :src="post.author.avatar_url" alt>
            </a>
            <span class="reply-visit-count">
                <span class="reply-count">{{post.reply_count}}</span>
                <span class="count-slash">/</span>
                <span class="visit-count">{{post.visit_count}}</span>
            </span>
            <span :class="[{classify: true, 'put-good': (post.good === true), 'put-top': (post.top === true), 
            'other-tab': (post.good !== true && post.top !== true)}]">
                {{post | tabFormatter}}
            </span >
              
                <span class="post-title">{{post.title}}</span>
              
            <span class="last-reply-at">{{post.last_reply_at | formatDate}}</span>
          </li>
          
        </ul>
      </div>
  </div>
</template>


<script>

export default {
  
  data() {
    return {
      isLoading: false,
      posts: [],
      postPage: 1
    };
  },
 
  methods: {
    getData() {
      this.$http
        .get("https://cnodejs.org/api/v1/topics", {
          params: {
            page: this.postPage,
            limit: 15
          }
        })
        .then(res => {
          this.isLoading = false; // 加载成功去除动画
          this.posts = res.data.data;
          console.log(res);
        })
        .catch(err => {
          console.log(err);
        });
    },
    renderList(value){
        this.postPage = value
        this.getData()
    }
  },
  beforeMount() {
    this.isLoading = true; // 在页面加载之前显示加载动画
    this.getData(); // 在页面加载之前获取数据
  }
};
</script>
<style scoped>
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
    width:100%;
    
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

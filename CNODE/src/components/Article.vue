<template>
  <div class="article">
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif">
    </div>

    <div v-else class="article-main">
      <div class="topic_header">
        <div class="topic_title">{{post.title}}</div>
        <ul>
          <li>发布于：{{post.create_at | formatDate}}</li>
          <li>作者：{{post.loginname}}</li>
          <li>{{post.visit_count}}次浏览</li>
          <li>来自{{post | tabFormatter}}</li>
        </ul>

        <div v-html="post.content" class="topic_content"></div>
      </div>

      <div class="topbar">回复</div>
      <div v-for="(reply,index) in post.replies ">
        <router-link :to="{name:'user_info',params:{ name:reply.author.loginname}}">
          <img :src="reply.author.avatar_url" alt>
        </router-link>

        <span>{{reply.author.loginname}}</span>
        <span>{{index + 1}}楼</span>
        <span v-if="reply.ups.length > 0">{{reply.ups.length}}赞</span>
        <span v-else></span>
        <p v-html="reply.content"></p>
      </div>
    </div>
  </div>
</template>






<script>
export default {
  data() {
    return {
      isLoading: false,
      post: {} //代表当前文章页的所有内容
    };
  },
  methods: {
    getArticleData() {
      this.$http
        .get("https://cnodejs.org/api/v1/topic/" + this.$route.params.id)
        .then(res => {
          if (res.data.success === true) {
            this.isLoading = false;
            this.post = res.data.data;
          }
        })
        .catch(function(err) {
          console.log(err);
        });
    }
  },
  beforeMount() {
    this.isLoading = true;
    this.getArticleData();
  }
};
</script>





<style >
@import url("../assets/markdown-github.css");

.topic_header {
  padding: 10px;
  background-color: #fff;
  border-bottom: 1px solid #f5f5f5;
  border-radius: 3px 3px 0 0;
}

.topic_title {
  font-size: 22px;
  font-weight: 700;
  max-width: 75%;
}

.topic_header > ul {
  display: flex;
}

.topic_header > ul > li {
  font-size: 12px;
  color: #838383;
  margin-top: 10px;
  margin-right: 4px;
}
</style>


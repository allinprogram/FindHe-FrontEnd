<template>
  <div class="flex flex-col justify-center my-4">
    <div>
      <h1 class="card-title">“{{ hitokoto }}”</h1>
      <div class="card-actions justify-end">
        <a v-bind:href="source" target="_blank">——《{{ name }}》{{ author }}</a>
      </div>
    </div>

    <div class="divider mt-8 font-bold" style="color: hsla(160, 100%, 37%, 1)">
      留言板
    </div>

    <p class="flex justify-center">欢迎你来到这里，同时也欢迎你在这里畅所欲言，分享自己的故事~</p>

    <!-- TODO 提交后数据插入现在的list -->
    <div class="container">
      <form @submit.prevent="submitComment">
        <label class="input input-bordered flex items-center mt-2">
          称呼
          <input v-model="nickname" type="text" class="grow ml-2" placeholder="将会被显示在留言板的名字。" required />
        </label>
        <label class="input input-bordered flex items-center mt-2">
          联系
          <input v-model="contact" type="text" class="grow ml-2" placeholder="若希望得到回复请输入有效联系方式。" required />
        </label>
        <label class="input input-bordered flex items-center mt-2">
          留言
          <input v-model="content" type="text" class="grow ml-2" placeholder="有什么想留下的话语吗？" required />
        </label>
        <div class="tooltip flex justify-end mt-2">
          <div class="tooltip-content">
            <div class="animate-bounce text-orange-400 -rotate-10 text-2xl font-black">Wow!</div>
          </div>
          <button class="btn btn-success" type="submit">提交</button>
        </div>
      </form>
    </div>

    <!-- 分割线 -->
    <div class="divide-y divide-dashed divide-gray-700">
      <div v-for="comment in commentList">
        <p class="text-lg text-success">{{ comment.nickname }}</p>
        <p class="mx-4">{{ comment.content }}</p>
        <time class="text-xs opacity-50 ml-2 flex justify-end">{{ formatTime(comment.publishTime) }}</time>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // 一言
      name: "",
      hitokoto: "",
      author: "",
      source: "",

      // 留言
      nickname: "",
      contact: "",
      content: "",
      commentList: [],
    };
  },
  created() {
    this.fetchHitokoto();
    this.fetchAllComment();
  },
  methods: {
    fetchHitokoto() {
      fetch("https://v1.hitokoto.cn/?c=i")
        .then((response) => response.json())
        .then((data) => {
          this.name = data.from;
          this.hitokoto = data.hitokoto;
          this.author = data.from_who;
          this.source += "https://hitokoto.cn/?uuid=" + data.uuid;
        })
        .catch((error) => {
          console.error("Error fetching quote:", error);
        });
    },
    fetchAllComment() {
      fetch("/api/comment")
        .then((response) => response.json())
        .then((result) => {
          this.commentList = result.data.reverse();
        })
        .catch((error) => {
          console.error("Error fetching comments:", error);
        });
    },
    async submitComment() {
      try {
        const response = await fetch("/api/comment", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            nickname: this.nickname,
            contact: this.contact,
            content: this.content,
          }),
        });
        this.response = await response.json();

        // 检查响应状态码，确保评论已成功提交
        if (!response.ok) {
          throw new Error("Failed to submit comment");
        }

        // 在commentList中添加新的评论
        this.commentList.unshift({
          publishTime: new Date().toLocaleString(),
          nickname: this.nickname,
          contact: this.contact,
          content: this.content,
        });

      } catch (error) {
        console.error(error);
        this.response = { code: 0, message: "发生异常，可以通过微信公众号进行反馈。" };
      }
    },
    formatTime(publishTime) {
      const now = new Date();
      const time = new Date(publishTime);
      const diff = now - time;
      const diffDays = Math.floor(diff / (1000 * 60 * 60 * 24));
      const diffHours = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
      if (diffDays > 0) {
        return `${diffDays}天${diffHours}小时前`;
      } else {
        return `${diffHours}小时前`;
      }
    },
  },
  mounted() { },
};
</script>

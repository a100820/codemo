<template>
  <div>
    <div class="time">
      {{ comment.time }}
      <span
        ><a href="javascript:void(0)" id="key" @click="showchildclick"
          >[-]</a
        ></span
      >
    </div>
    <div class="main">{{ comment.comment }}</div>
    <div class="reply">
      <a href="javascript:void(0)" @click="replyclick">reply</a>
    </div>
    <div class="replayarea" v-show="isshow">
      <textarea
        placeholder="请输入回复"
        class="replytext"
        v-model="curryreply"
      ></textarea>
      <button @click="replybutton">确认回复</button>
      <button @click="debutton">取消回复</button>
    </div>

    <!-- 子级 -->
    <div
      v-for="(value, key) in childcomments"
      :key="key"
      class="childreply"
      v-show="showchild"
    >
      <coment :comment="value" :comments="comments"></coment>
    </div>
  </div>
</template>

<script>
export default {
  name: "coment",
  props: {
    comment: {
      type: Object,
      default() {
        return {};
      },
    },
    comments: {
      type: Array,
      default() {
        return [];
      },
    },
  },
  data() {
    return {
      isshow: false,
      curryreply: "",
      showchild: true,
      childcomments: [],
    };
  },
  methods: {
    replyclick() {
      this.isshow = true;
    },
    debutton() {
      this.isshow = false;
    },
    showchildclick() {
      this.showchild = !this.showchild;
    },
    replybutton() {
      if (!this.curryreply) {
        alert("请输入回复");
      } else {
        let time = new Date().toLocaleString();
        let curryreply = {
          time: time,
          comment: this.curryreply,
          children: [],
        };
        // this.childcomments = this.comment.children;
        // this.comment.children.push(curryreply);
        this.childcomments.push(curryreply);

        this.curryreply = "";
        this.isshow = false;
        alert("回复成功");
      }
    },
  },
  mounted() {
    this.childcomments = this.comment.children;
  },
  updated() {
    //每次添加回复存到 sessionStorage
    this.comments.forEach(function (value) {
      sessionStorage.setItem(value.time, JSON.stringify(value));
    });
  },
};
</script>

<style>
.childreply {
  padding-left: 15px;
}
</style>
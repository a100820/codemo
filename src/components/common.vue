<template>
  <div>
    <div>
      <textarea placeholder="请输入评论" class="text" v-model="curryvalue">
      </textarea>
    </div>
    <div><button @click="addclick">add comment</button></div>
    <div v-for="(value, key) in comments" :key="key">
      <coment :comment="value" :comments="comments"></coment>
    </div>
  </div>
</template>

<script>
import coment from "./coment.vue";
export default {
  name: "common",
  components: {
    coment,
  },
  data() {
    return {
      curryvalue: "",
      comments: [],
      curryreply: "",
    };
  },
  methods: {
    addclick() {
      if (!this.curryvalue) {
        alert("请输入内容");
      } else {
        let time = new Date().toLocaleString();
        let currycomment = {
          time: time,
          comment: this.curryvalue,
          children: [],
        };
        //用时间当key
        sessionStorage.setItem(time, JSON.stringify(currycomment));
        this.comments.push(JSON.parse(sessionStorage.getItem(time)));
        alert("添加成功");
        this.curryvalue = "";
      }
    },
    //点击回复按钮
    // replyclick(value) {
    //   console.log("kankan");
    //   console.log(value);
    //   this.isshow = !this.isshow;
    // },
  },
  mounted() {
    var sessionKeys = Object.keys(sessionStorage);

    for (var i = 0; i < sessionKeys.length; i++) {
      //把缓存放到comments
      this.comments.push(JSON.parse(sessionStorage.getItem(sessionKeys[i])));
    }
  },
};
</script>

<style >
* {
  margin: 0;
  padding: 0;
}
a {
  color: gray;
}
li {
  list-style: none;
  margin-top: 15px;
}
.text {
  height: 200px;
  width: 500px;
}
</style>
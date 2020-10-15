<template>
  <div class="hello">
    <van-nav-bar title="编辑问卷" left-text="返回" left-arrow @click-right="onClickPlus">
      <template #right>
        <van-icon name="plus" size="30" />
      </template>
    </van-nav-bar>

    <van-row type="flex" justify="center">
      <van-col span="10">
        <van-button size="large" plain hairline type="primary">实名</van-button>
      </van-col>
      <van-col span="10">
        <van-button size="large" plain hairline type="info">匿名</van-button>
      </van-col>
    </van-row>
    <QuestionItem
      v-for="(item,index) in items"
      :key="index"
      :index="index"
      :items="items"
      @deleteIndex="del"
      @uploadData="getData"
    ></QuestionItem>
    <!-- <ShareImg class="share-block"></ShareImg> -->
    <van-row type="flex" justify="center">
      <van-button class="share-block" size="large" type="info" @click="getListData">提交</van-button>
    </van-row>
  </div>
</template>

<script>
import QuestionItem from "./QuestionItem.vue";
import ShareImg from "./ShareImg.vue";

import Vue from "vue";
import { Col, Row } from "vant";
import { Button, Icon } from "vant";
import { Field } from "vant";
import { NavBar } from "vant";
import VueResource from "vue-resource";
Vue.use(VueResource);
Vue.use(ShareImg);
Vue.use(NavBar);
Vue.use(Field);
Vue.use(Button);
Vue.use(Col);
Vue.use(Row);
Vue.use(Icon);
export default {
  components: {
    QuestionItem
    // ShareImg
  },
  props: {
    msg: String
  },
  data() {
    return {
      items: [{}]
    };
  },
  methods: {
    onClickPlus: function() {
      this.items.push({ question: "" });
    },
    del: function(index) {
      //  not allow to delete the first
      if (index !== 0) {
        this.items.splice(index, 1);
      }
    },
    //  get the data from child
    getData: function(val) {
      let index = val.index;
      this.items[index] = val.data;
    },

    getListData() {
      //网络请求数据
      var url = "/api/question/submit";
      //jsonp请求，需要后台接口支持jsonp
      // this.$http.jsonp(api).then((response)=>{
      //post请求
      var total = this.items.length;
      var questions = [];
      for (let i = 0; i < this.items.length; i++) {
        const element = JSON.stringify(this.items[i]);
        var question = { id: 0, question: "" };
        question.id = i;
        question.question = element;
        questions.push(question);
      }

      this.$http.post(url, { total: total, questions: questions }).then(
        response => {
          console.log("请求到的数据：" + JSON.stringify(response.body));
          this.list = response.body.result;
        },
        error => {
          console.log("请求错误：" + error);
        }
      );
    }
  }
};
</script>

<style scoped>
.share-block {
  position: fixed;
  bottom: 0px;
  width: 100%;
}
</style>

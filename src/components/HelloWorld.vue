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
    <ShareImg class="share-block"></ShareImg>
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

Vue.use(ShareImg);
Vue.use(NavBar);
Vue.use(Field);
Vue.use(Button);
Vue.use(Col);
Vue.use(Row);
Vue.use(Icon);
export default {
  components: {
    QuestionItem,
    ShareImg
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
        console.log("deleted:", JSON.stringify(this.items));
      }
    },
    //  get the data from child
    getData: function(val) {
      let index = val.index;
      this.items[index] = val.data;
      console.log("I got the data:", JSON.stringify(this.items));
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

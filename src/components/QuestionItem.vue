<template>
  <div>
    <van-row>
      <van-col span="4">
        <van-cell>问题{{index+1}}</van-cell>
      </van-col>
      <van-col span="20">
        <van-swipe-cell>
          <van-field v-model="question.question" placeholder="Please enter question" clearable />
          <template #right>
            <van-button square type="danger" text="删除" @click="deleteQuestion" />
          </template>
        </van-swipe-cell>
      </van-col>
    </van-row>
  </div>
</template>
<script>
import Vue from "vue";
import { Col, Row } from "vant";
import { Button } from "vant";
import { Cell } from "vant";
import { SwipeCell } from "vant";

Vue.use(SwipeCell);
Vue.use(Cell);
Vue.use(Button);
Vue.use(Col);
Vue.use(Row);
export default {
  props: {
    index: {
      type: Number,
      required: true
    },
    items: {
      type: Array,
      default: Array
    }
  },
  data() {
    return {
      question: {
        question: ""
      }
    };
  },
  watch: {
    question: {
      handler(newV) {
        if (newV.question === "") {
          return false;
        }
        this.$emit("uploadData", { index: this.index, data: newV });
      },
      deep: true
    },
    items: {
      handler(newV) {
        if (newV.length !== 0) {
          this.question = { ...newV[this.index] };
        }
      },
      deep: true
    }
  },
  methods: {
    deleteQuestion: function() {
      this.$emit("deleteIndex", this.index);
    }
  }
};
</script>
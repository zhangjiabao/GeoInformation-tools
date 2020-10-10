<template>
  <div bottom="0">
    <van-cell title="分享给同学" @click="showShare = true" />
    <van-share-sheet v-model="showShare" title="分享给同学" :options="options" @select="onSelect" />
  </div>
</template>
<script>
import Vue from "vue";
import { ShareSheet } from "vant";
import { Toast } from "vant";
Vue.use(ShareSheet);

export default {
  data() {
    return {
      showShare: false,
      options: [
        { name: "微信", icon: "wechat" },
        // { name: '微博', icon: 'weibo' },
        // { name: '复制链接', icon: 'link' },
        // { name: '分享海报', icon: 'poster' },
        { name: "二维码", icon: "qrcode" }
      ]
    };
  },
  methods: {
    onSelect(option) {
      Toast(option.name);
      this.showShare = false;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
<template>
  <div class="scroll-container" @scroll="handleScroll">
    <div class="list" v-for="(item, index) in list" :key="index" :style="{background: item.bk}">{{ index + 1 }}</div>
    <Loading v-if="isLoading"/>
  </div>
</template>

<script lang="ts">
import Loading from './Loading.vue'
function generateRandomColorRGBA() {
  var color = 'rgb(';
  for (var i = 0; i < 2; i++) {
    color += Math.floor(Math.random() * 256) + ',';
  }
  color += Math.floor(Math.random() * 1) + ')';
  return color;
}
export default {
  components: {
    Loading
  },
  data() {
    return {
      list: [{ bk: "rgb(233,32,38)" }, { bk: "rgb(20,32,88)" }], // 存储列表数据
      isLoading: false, // 是否正在加载数据
    };
  },
  methods: {
    getList(num = 0) {
      // 生成n个长度的数组
      return new Promise((resolve) => {
          setTimeout(() => {
            // this.$data.isLoading = false
            const newList = Array.from({ length: num }, () => {
              return {bk: generateRandomColorRGBA()}
            })
            resolve(newList)
          }, 1000 * 2)
      })
    },
    handleScroll(event) {
      // 渲染超过50个停止
      if (this.list.length > 50) return
      if (this.isLoading) return
      const container = event.target;
      if (container.scrollHeight - container.scrollTop === container.clientHeight){
        // 滚动加载
        this.isLoading = true
        this.getList(2).then((newList) => {
          this.list = this.list.concat(newList)
          this.isLoading = false
        })
      }
    },
  },
};
</script>

<style scoped>
.scroll-container {
  width:100%;
  height: 100vh;
  overflow-y: scroll;
  text-align: center;
  line-height: 500px;
  font-size: 200px;
}
.scroll-container .list {
  width: 100%;
  height: 500px;
  margin-bottom: 10px;
}
.scroll-container .list div::last-child {
  width: 100%;
  height: 500px;
  margin-bottom: none !important;
}

</style>
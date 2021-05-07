<template>
  <transition name="slide-right">
    <!-- 章节菜单 -->
    <div class="content" v-show="contentShow">
      <div class="wrapper" v-if="bookAvilable">
        <div
          class="content-item"
          v-for="(item, index) in navigation.toc"
          :key="index"
          @click="jumpTo(item.href)"
        >
          <p class="text">{{ item.label }}</p>
        </div>
      </div>
      <div class="empty" v-else>加载中...</div>
    </div>
  </transition>
</template>

<script>
export default {
  props: {
    navigation: Object,
    // 书籍是否渲染完
    bookAvilable: Boolean,
    contentShow: Boolean
  },
  methods: {
    // 点击跳转
    jumpTo(target) {
      this.$emit('jumpTo', target)
    }
  }
}
</script>

<style lang="scss" scoped>
@import "../styles/global.scss";
.content {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: px2rem(100);
  background-color: #fff;
  z-index: 9999;
  .wrapper {
    display: flex;
    flex-direction: column;
    height: 100%;
    .content-item {
      flex: 1;
      display: flex;
      @include center;
      width: 100%;
      border-bottom: 1px solid #ccc;
    }
  }
}
</style>

<template>
  <transition name="slide-down">
    <!-- 顶部菜单 -->
    <div class="title" v-show="menuShow">
      <div class="title-left">
        <span class="iconfont">&#xe62e;</span>
      </div>
      <ul class="title-right">
        <li class="iconfont">&#xe61d;</li>
        <li class="iconfont">&#xe64e;</li>
        <li class="iconfont">&#xe65b;</li>
      </ul>
    </div>
  </transition>
</template>

<script>
export default {
  props: {
    menuShow: {
      type: Boolean,
      default: false
    }
  }
}
</script>

<style lang="scss" scoped>
@import "../styles/global";

.title {
  position: absolute;
  display: flex;
  top: 0;
  left: 0;
  width: 100%;
  height: px2rem(15);
  z-index: 999;
  background-color: #fff;
  box-shadow: 0 px2rem(2) px2rem(2) rgba(0, 0, 0, 0.5);
  &-left {
    flex: 0 0 px2rem(20);
    @include center;
    .iconfont {
      font-size: px2rem(8);
    }
  }
  &-right {
    flex: 1;
    display: flex;

    justify-content: flex-end;
    align-items: center;
    .iconfont {
      flex: 0 0 px2rem(15);
      font-size: px2rem(8);
    }
  }
}
</style>

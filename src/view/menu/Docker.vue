<template>
  <transition name="slide-up">
    <div
      class="docker"
      :class="{ 'hide-box-shadow': fontSetting || !menuShow }"
      v-show="menuShow"
    >
      <ul class="docker-icon">
        <li class="iconfont" @click="fontShow(3)">&#xe937;</li>
        <li class="iconfont" @click="fontShow(2)">&#xe600;</li>
        <li class="iconfont" @click="fontShow(1)">&#xe834;</li>
        <li class="iconfont" @click="fontShow(0)">A</li>
      </ul>
    </div>
  </transition>
</template>

<script>
export default {
  props: {
    menuShow: Boolean,
    fontSetting: Boolean
  },
  methods: {
    fontShow(tag) {
      this.$emit('fontShow', tag)
    }
  }
}
</script>

<style lang="scss" scoped>
@import "../../styles/global";
.docker {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: px2rem(15);
  z-index: 999;
  background-color: #fff;
  box-shadow: 0 px2rem(-2) px2rem(2) rgba(0, 0, 0, 0.5);
  &-icon {
    align-items: center;
    display: flex;

    .iconfont {
      display: flex;
      flex: 1;
      @include center;
      font-size: px2rem(8);
      line-height: px2rem(15);
    }
  }
}
</style>

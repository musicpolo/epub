<template>
  <section class="title">
    <!-- 底部菜单 -->
    <Docker
      :fontSetting="fontSetting"
      :menuShow="menuShow"
      @fontShow="fontShow"
    />
    <!-- 设置项 -->
    <Set
      :fontSetting="fontSetting"
      :showTag="showTag"
      :fontSizeList="fontSizeList"
      :defaultFontSize="defaultFontSize"
      :themeList="themeList"
      :defaultTheme="defaultTheme"
      :bookAvilable="bookAvilable"
      @setFontSize="setFontSize"
      @onProgressChange="onProgressChange"
      @setTheme="setTheme"
    />
    <!-- 章节菜单 -->
    <Content
      :contentShow="contentShow"
      :bookAvilable="bookAvilable"
      :navigation="navigation"
      @jumpTo="jumpTo"
    />
    <!-- 蒙板 -->
    <transition name="fade">
      <div class="content-mask" v-show="contentShow" @click="hideContent"></div>
    </transition>
  </section>
</template>

<script>
import Content from '../../components/Content.vue'
import Docker from './Docker'
import Set from './Set.vue'
export default {
  components: { Content, Docker, Set },
  props: [
    'menuShow',
    'fontSizeList',
    'defaultFontSize',
    'themeList',
    'defaultTheme',
    'bookAvilable',
    'navigation'
  ],
  data() {
    return {
      fontSetting: false,
      showTag: 0,
      contentShow: false
    }
  },
  methods: {
    // 章节跳转
    jumpTo(target) {
      this.$emit('jumpTo', target)
    },
    // 隐藏章节菜单
    hideContent() {
      this.contentShow = false
    },
    // 进度条
    onProgressChange(progress) {
      this.$emit('onProgressChange', progress)
    },
    // 设置主题
    setTheme(index) {
      this.$emit('setTheme', index)
    },
    // 设置项切换
    fontShow(tag) {
      this.showTag = tag
      if (this.showTag === 3) {
        this.fontSetting = false
        this.contentShow = true
      } else {
        this.fontSetting = true
      }
    },
    // 隐藏设置项
    hideSetting() {
      this.fontSetting = false
    },
    // 设置字体大小
    setFontSize(fontSize) {
      this.$emit('setFontSize', fontSize)
    }
  }
}
</script>

<style lang="scss" scoped>
@import "../../styles/global.scss";
.title {
  .setting {
    position: absolute;
    height: px2rem(20);
    bottom: px2rem(15);
    left: 0;
    width: 100%;
    background-color: #fff;
    z-index: 999;
    box-shadow: 0 px2rem(-2) px2rem(2) rgba(0, 0, 0, 0.5);
    .font-setting {
      height: 100%;
      align-items: center;
      display: flex;
      z-index: 99;
      &-prev {
        &:first-child {
          display: flex;
          justify-content: flex-end;
        }
        flex: 0 0 px2rem(25);
      }
      &-select {
        flex: 1;
        display: flex;
        .item {
          &:first-child {
            .item-line {
              &:first-child {
                border-top: none;
              }
            }
          }
          &:last-child {
            .item-line {
              &:last-child {
                border-top: none;
              }
            }
          }
          flex: 1;
          display: flex;
          align-items: center;
          &-line {
            height: 0;
            flex: 1;
            border-top: px2rem(0.5) solid #ccc;
          }
          &-point {
            position: relative;
            flex: 0 0 0;
            width: 0;
            height: px2rem(4);
            border-left: px2rem(0.5) solid #ccc;
            &-circle {
              position: absolute;
              top: px2rem(-0.7);
              left: px2rem(-2.8);
              width: px2rem(5);
              height: px2rem(5);
              border-radius: 50%;
              background-color: #fff;
              box-shadow: 0 px2rem(1) px2rem(1) rgba(0, 0, 0, 0.5);
              .circle-small {
                margin-top: 50%;
                margin-left: 50%;
                transform: translate3d(px2rem(-0.5), px2rem(-0.5), 0);
                width: px2rem(1);
                height: px2rem(1);
                border-radius: 50%;
                background-color: #000;
              }
            }
          }
        }
      }
    }
    &-theme {
      height: 100%;
      .theme {
        height: 100%;
        display: flex;
        &-item {
          flex: 1;
          display: flex;
          flex-direction: column;
          align-items: center;
          justify-content: center;
          padding: px2rem(2);
          box-sizing: border-box;
          &-box {
            flex: 1;
            width: 100%;
            border: px2rem(0.1) solid #ccc;
          }
          .no-border {
            border: none;
          }
          &-name {
            flex: 0 0 px2rem(10);
            font-size: px2rem(5);
            @include center;
            color: #ccc;
          }
          .selected {
            color: #000;
          }
        }
      }
    }
    &-progress {
      width: 100%;
      height: 100%;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      &-input {
        width: 80%;
      }
    }
  }
  .hide-box-shadow {
    box-shadow: none;
  }
  .content-mask {
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    background-color: rgba(0, 0, 0, 0.8);
    z-index: 999;
  }
}
</style>

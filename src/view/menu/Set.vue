<template>
  <transition name="slide-up">
    <!-- 设置 -->
    <div class="setting" v-show="fontSetting">
      <!-- 设置字体大小 -->
      <div class="font-setting" v-if="showTag === 0">
        <div
          class="font-setting-prev"
          :style="{ fontSize: fontSizeList[0].fontSize + 'px' }"
        >
          A
        </div>
        <ul class="font-setting-select">
          <li
            class="item"
            v-for="(item, index) in fontSizeList"
            :key="index"
            @click="setFontSize(item.fontSize)"
          >
            <p class="item-line"></p>
            <div class="item-point">
              <div
                class="item-point-circle"
                v-show="defaultFontSize === item.fontSize"
              >
                <div class="circle-small"></div>
              </div>
            </div>
            <p class="item-line"></p>
          </li>
        </ul>
        <div
          class="font-setting-prev"
          :style="{
            fontSize: fontSizeList[fontSizeList.length - 1].fontSize + 'px',
          }"
        >
          A
        </div>
      </div>
      <!-- 设置主题 -->
      <div class="setting-theme" v-else-if="showTag === 1">
        <ul class="theme">
          <li
            class="theme-item"
            v-for="(item, index) in themeList"
            :key="index"
            @click="setTheme(index)"
          >
            <div
              class="theme-item-box"
              :class="{ 'no-border': item.style.body.background !== '#fff' }"
              :style="{ background: item.style.body.background }"
            ></div>
            <div
              class="theme-item-name"
              :class="{ selected: index === defaultTheme }"
            >
              {{ item.name }}
            </div>
          </li>
        </ul>
      </div>
      <!-- 设置进度条 -->
      <div class="setting-progress" v-else-if="showTag === 2">
        <input
          class="setting-progress-input"
          type="range"
          max="100"
          min="1"
          step="1"
          @change="onProgressChange($event.target.value)"
          @input="onProgressInput($event.target.value)"
          :value="progress"
          :disabled="!bookAvilable"
          ref="progress"
        />
        <p class="setting-progress-text">
          {{ bookAvilable ? progress + "%" : "加载中..." }}
        </p>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  props: [
    'fontSetting',
    'showTag',
    'fontSizeList',
    'defaultFontSize',
    'themeList',
    'bookAvilable',
    'defaultTheme'
  ],
  data() {
    return {
      progress: 0
    }
  },
  methods: {
    // 设置字体
    setFontSize(fontSize) {
      this.$emit('setFontSize', fontSize)
    },
    // 进度条数字改变
    onProgressInput(progress) {
      this.progress = progress
      this.$refs.progress.style.backgroundSize = `${this.progress}% 100%`
    },
    // 进度条改变事件
    onProgressChange(progress) {
      this.$emit('onProgressChange', progress)
    },
    // 设置主题
    setTheme(index) {
      this.$emit('setTheme', index)
    }
  }
}
</script>

<style lang="scss" scoped>
@import "../../styles/global";
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
</style>

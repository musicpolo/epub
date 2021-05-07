<template>
  <section class="ebook">
    <!-- 顶部菜单 -->
    <Title :menuShow="menuShow" />
    <div class="wrapper">
      <div class="mask">
        <div class="left" @click="prePage"></div>
        <div class="center" @click="toggoleMenu"></div>
        <div class="right" @click="nextPage"></div>
      </div>
      <div id="read"></div>
      <!-- 底部菜单 -->
      <Menu
        :menuShow="menuShow"
        ref="menuBar"
        :fontSizeList="fontSizeList"
        :defaultFontSize="defaultFontSize"
        @setFontSize="setFontSize"
        :themeList="themeList"
        :defaultTheme="defaultTheme"
        @setTheme="setTheme"
        :bookAvilable="bookAvilable"
        @onProgressChange="onProgressChange"
        @jumpTo="jumpTo"
        :navigation="navigation"
      />
    </div>
  </section>
</template>

<script>
import Epub from 'epubjs'
import Menu from './view/menu/Menu'
import Title from './components/Title'
const DOWNLOAD_URL = '/static/qun.epub'
export default {
  components: {
    Menu,
    Title
  },
  data() {
    return {
      menuShow: false,
      fontSizeList: [
        { fontSize: 14 },
        { fontSize: 16 },
        { fontSize: 18 },
        { fontSize: 20 },
        { fontSize: 22 },
        { fontSize: 24 },
        { fontSize: 26 }
      ],
      defaultFontSize: 16,
      themeList: [
        {
          name: 'default',
          style: {
            body: {
              color: '#000',
              background: '#fff'
            }
          }
        },
        {
          name: 'eye',
          style: {
            body: {
              color: '#000',
              background: '#ceeaba'
            }
          }
        },
        {
          name: 'night',
          style: {
            body: {
              color: '#fff',
              background: '#333'
            }
          }
        },
        {
          name: 'gold',
          style: {
            body: {
              color: '#000',
              background: '#ecc'
            }
          }
        }
      ],
      defaultTheme: 0,
      bookAvilable: false,
      navigation: {}
    }
  },
  methods: {
    // 跳转章节
    jumpTo(href) {
      this.rendition.display(href)
      this.hideMenu()
    },
    // 隐藏菜单
    hideMenu() {
      this.menuShow = false
      this.$refs.menuBar.hideSetting()
      this.$refs.menuBar.hideContent()
    },
    // 进度条
    onProgressChange(progress) {
      const percentage = progress / 100
      const locations =
        percentage > 0 ? this.locations.cfiFromPercentage(percentage) : 0
      this.rendition.display(locations)
    },
    // 注册主题
    registerTheme() {
      this.themeList.forEach(theme => {
        this.theme.register(theme.name, theme.style)
      })
    },
    // 设置主题
    setTheme(index) {
      this.theme.select(this.themeList[index].name)
      this.defaultTheme = index
    },
    // 设置字体
    setFontSize(fontSize) {
      this.defaultFontSize = fontSize
      if (this.theme) {
        this.theme.fontSize(fontSize + 'px')
      }
    },
    // 设置菜单切换
    toggoleMenu() {
      this.menuShow = !this.menuShow

      if (!this.menuShow) {
        this.$refs.menuBar.hideSetting()
      }
    },
    // 前一页
    prePage() {
      if (this.rendition) {
        this.rendition.prev()
      }
    },
    // 后一页
    nextPage() {
      if (this.rendition) {
        this.rendition.next()
      }
    },
    // 渲染书籍
    showEpub() {
      this.book = new Epub(DOWNLOAD_URL)
      // 获取rendition
      this.rendition = this.book.renderTo('read', {
        width: window.innerWidth,
        height: window.innerHeight
      })
      // 渲染
      this.rendition.display()
      // 获取主题
      this.theme = this.rendition.themes
      this.setFontSize(this.defaultFontSize)
      this.registerTheme()
      // 通过epubjs钩子函数获取进度、章节
      this.book.ready
        .then(() => {
          this.navigation = this.book.navigation
          console.log(this.navigation)
          return this.book.locations.generate()
        })
        .then(res => {
          this.locations = this.book.locations
          this.bookAvilable = true
        })
    }
  },
  mounted() {
    this.showEpub()
  }
}
</script>

<style lang='scss' scoped>
@import "./styles/global";
.ebook {
  position: relative;

  .wrapper {
    .mask {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      display: flex;
      z-index: 9;
      .left {
        flex: 0 0 px2rem(100);
      }
      .center {
        flex: 1;
      }
      .right {
        flex: 0 0 px2rem(100);
      }
    }
  }
}
</style>

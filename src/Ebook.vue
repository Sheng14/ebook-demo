<template>
  <div class="ebook">
      <TitleBar :isTitleAndMenuShow = "isTitleAndMenuShow"></TitleBar>
      <div class="read-wrapper">
          <div id="read"></div>
          <div class="mask">
            <div class="left" @click="prevPage"></div>
            <div class="center" @click="toggleTitleMenu"></div>
            <div class="right" @click="nextPage"></div>
          </div>
      </div>
      <MenuBar :isTitleAndMenuShow = "isTitleAndMenuShow"
      ref="menuBar"
      :fontSizeList = "fontSizeList"
      :defaultFontSize = "defaultFontSize"
      @setFontSize = "setFontSize"
      :themeList = "themeList"
      :defaultTheme = "defaultTheme"
      @setTheme = "setTheme"></MenuBar>
  </div>
</template>

<script>
import Epub from 'epubjs' // 导入epubjs库
import MenuBar from './components/MenuBar'
import TitleBar from './components/TitleBar'
const DOWNLOAD = '/static/2015_Book_ProtectingTheRightsOfPeopleWit.epub' // 定义要使用图书的路径
export default {
  components: {
    MenuBar,
    TitleBar
  },
  data () {
    return {
      isTitleAndMenuShow: false,
      fontSizeList: [ // 字号集
        { fontSize: 12 },
        { fontSize: 14 },
        { fontSize: 16 },
        { fontSize: 18 },
        { fontSize: 20 },
        { fontSize: 22 },
        { fontSize: 24 }
      ],
      defaultFontSize: 16, // 默认字号
      themeList: [
        {
          name: 'default',
          style: {
            body: {
              'color': '#000',
              'background': '#fff'
            }
          }
        },
        {
          name: 'eye',
          style: {
            body: {
              'color': '#000',
              'background': '#ceeaba'
            }
          }
        },
        {
          name: 'night',
          style: {
            body: {
              'color': '#fff',
              'background': '#000'
            }
          }
        },
        {
          name: 'gold',
          style: {
            body: {
              'color': '#000',
              'background': 'rgb(241, 236, 226)'
            }
          }
        }
      ],
      defaultTheme: 0 // 默认主题的索引
    }
  },
  methods: {
    setTheme (index) { // 根据索引选择主题
      this.theme.select(this.themeList[index].name)
      this.defaultTheme = index // 同时选择后的主题即作为默认主题，方便缓存以为下次启动时的主题
    },
    registerTheme () { // 遍历注册每一个主题
      this.themeList.forEach(theme => {
        this.theme.register(theme.name, theme.style)
      })
    },
    setFontSize (fontSize) { // 接收到子组件传来的方法设置字号大小
      this.defaultFontSize = fontSize // 改变默认字号才能让小球移动
      if (this.theme) {
        this.theme.fontSize(fontSize + 'px') // 记得加px！
      }
    },
    showEpub () {
      this.book = new Epub(DOWNLOAD) // 生成epub对象
      this.rendition = this.book.renderTo('read', {
        width: innerWidth,
        height: innerHeight
      }) // 生成rendition对象，挂载到id为read的DOM对象
      this.rendition.display() // 渲染电子书
      this.theme = this.rendition.themes // 拿到电子书的模式
      this.registerTheme() // 调用注册主题的方法
      this.setTheme(this.defaultTheme) // 调用默认的主题
    }, // 其实这里应该用this而不是let，用this挂载到全局上面才能被别的地方调用访问，否则如prevPage是访问不到rendition！
    prevPage () { // 上一页
      if (this.rendition) {
        this.rendition.prev()
      }
    },
    nextPage () { // 下一页
      if (this.rendition) {
        this.rendition.next()
      }
    },
    toggleTitleMenu () {
      this.isTitleAndMenuShow = !this.isTitleAndMenuShow
      if (!this.isTitleAndMenuShow) {
        this.$refs.menuBar.hideSetting() // 如果我们底部框框要隐藏了，那么我们的设置框框也应该被调用子组件方法隐藏掉
      }
    }
  },
  mounted () {
    this.showEpub()
  }
}
</script>

<style lang='scss' scoped>
@import 'assets/styles/global';
.ebook {
    position: relative;
    .read-wrapper {
        .mask {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 100;
            display: flex;
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

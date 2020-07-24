<template>
  <div class="ebook">
    <transition name="slide-down">
      <div class="title-wrapper" v-show="isTitleAndMenuShow">
          <div class="left">
              <div class="icon-back icon"></div>
          </div>
          <div class="right">
              <div class="icon-wrapper">
                  <span class="icon-cart icon"></span>
              </div>
              <div class="icon-wrapper">
                  <span class="icon-person icon"></span>
              </div>
              <div class="icon-wrapper">
                  <span class="icon-more icon"></span>
              </div>
          </div>
      </div>
    </transition>
      <div class="read-wrapper">
          <div id="read"></div>
          <div class="mask">
            <div class="left" @click="prevPage"></div>
            <div class="center" @click="toggleTitleMenu"></div>
            <div class="right" @click="nextPage"></div>
          </div>
      </div>
    <transition name="slide-up">
      <div class="menu-wrapper"  v-show="isTitleAndMenuShow">
          <div class="icon-wrapper">
              <span class="icon-menu icon"></span>
          </div>
          <div class="icon-wrapper">
              <span class="icon-progress icon"></span>
          </div>
          <div class="icon-wrapper">
              <span class="icon-bright icon"></span>
          </div>
          <div class="icon-wrapper">
              <span class="icon-a icon">A</span>
          </div>
      </div>
    </transition>
  </div>
</template>

<script>
import Epub from 'epubjs' // 导入epubjs库
const DOWNLOAD = '/static/2015_Book_ProtectingTheRightsOfPeopleWit.epub' // 定义要使用图书的路径
export default {
  data () {
    return {
      isTitleAndMenuShow: false
    }
  },
  methods: {
    showEpub () {
      this.book = new Epub(DOWNLOAD) // 生成epub对象
      this.rendition = this.book.renderTo('read', {
        width: innerWidth,
        height: innerHeight
      }) // 生成rendition对象，挂载到id为read的DOM对象
      this.rendition.display() // 渲染电子书
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
    .title-wrapper {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: px2rem(48);
        z-index: 101;
        box-shadow: 0 px2rem(8) px2rem(8) rgba(0, 0, 0, .15);
        background-color: white;
        display: flex;
        .left {
            flex: 0 0 px2rem(60);
            @include center;
        }
        .right {
            flex: 1;
            display: flex;
            justify-content: flex-end;
            .icon-wrapper {
                flex: 0 0 px2rem(40);
            }
        }
        &.slide-down-enter,&.slide-down-leave-to {
          transform: translate3d(0, -100%, 0); // 从上往下进入，进入前应该在外面即上移所以为负
        }
        &.slide-down-enter-to,&.slide-down-leave {
          transform: translate3d(0, 0, 0); // 进入后回归原位所以为0
        }
        &.slide-down-enter-active,&.slide-down-leave-active {
            transition: all 0.3s linear;
        }
    }
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
    .menu-wrapper {
        position: absolute;
        bottom: 0;
        left: 0;
        width: 100%;
        height: px2rem(48);
        z-index: 101;
        background-color: white;
        display: flex;
        box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, .15);
        .icon-wrapper {
            flex: 1;
            @include center;
        }
        &.slide-up-enter,&.slide-up-leave-to {
            transform: translate3d(0, 100%, 0); // 从下往上进入，进入前应该在外面所以下移故为正，坐标轴是↓→
        }
        &.slide-up-enter-to,&.slide-up-leave {
            transform: translate3d(0, 0, 0);
        }
        &.slide-up-enter-active,&.slide-up-leave-active {
            transition: all 0.3s linear;
        }
    }
}
</style>

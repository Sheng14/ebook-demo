<template>
    <div class="menu-bar">
        <transition name="slide-up">
        <div class="menu-wrapper"  v-show="isTitleAndMenuShow" :class="{'hide-box-shadow':!isTitleAndMenuShow || isSettingShow}">
            <div class="icon-wrapper">
                <span class="icon-menu icon"></span>
            </div>
            <div class="icon-wrapper">
                <span class="icon-progress icon"></span>
            </div>
            <div class="icon-wrapper" @click="showSetting(1)">
                <span class="icon-bright icon"></span>
            </div>
            <div class="icon-wrapper" @click="showSetting(0)">
                <span class="icon-a icon">A</span>
            </div>
        </div>
        </transition>
        <transition name="slide-up">
            <div class="setting-bar" v-show="isSettingShow">
                <div class="setting-font-size" v-if="showTag === 0">
                    <div class="preview" :style="{fontSize: fontSizeList[0].fontSize + 'px'}">A</div>
                    <div class="select">
                        <div class="select-wrapper"
                            v-for="(item, index) in fontSizeList" :key="index"
                            @click="setFontSize(item.fontSize)"
                        >
                            <div class="line"></div>
                            <div class="point-wrapper">
                                <div class="point" v-show="defaultFontSize === item.fontSize">
                                    <div class="small-point"></div>
                                </div>
                            </div>
                            <div class="line"></div>
                        </div>
                    </div>
                    <div class="preview" :style="{fontSize: fontSizeList[fontSizeList.length-1].fontSize + 'px'}">A</div>
                </div>
                <div class="setting-theme" v-else-if="showTag === 1">
                    <div class="setting-theme-item" v-for="(item, index) in themeList" :key="index" @click="setTheme(index)">
                        <div class="preview"
                        :style="{background: item.style.body.background}"
                        :class="{'have-border': item.style.body.background === '#fff'}"></div>
                        <div class="text" :class="{'selected': index === defaultTheme}">{{item.name}}</div>
                    </div>
                </div>
            </div>
        </transition>
    </div>
</template>

<script>
export default {
  props: {
    isTitleAndMenuShow: { // 是否显示菜单栏
      type: Boolean,
      default: false
    },
    fontSizeList: Array,
    defaultFontSize: Number,
    themeList: Array
  },

  data () {
    return {
      isSettingShow: false,
      showTag: 0, // 表示要展示哪一部分，默认是字号
      defaultTheme: 0
    }
  },
  methods: {
    setTheme (index) { // 修改主题
      this.$emit('setTheme', index) // 传递给父组件让它改
      this.defaultTheme = index // 既然主题被修改了，那么这个被修改的就应该是变为默认的主题
    },
    showSetting (tag) { // 点击显示设置框框
      this.isSettingShow = true
      this.showTag = tag
      console.log(this.fontSizeList[0].fontSize)
    },
    hideSetting () { // 隐藏设置框框（留给父组件调用的）
      this.isSettingShow = false
    },
    setFontSize (fontSize) { // 设置字号大小（注：需要传给父组件调用theme对象里面的方法才能修改）
      this.$emit('setFontSize', fontSize)
    }
  }
}
</script>

<style lang='scss' scoped>
@import '../assets/styles/global';
.menu-bar {
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
            transform: translate3d(0, px2rem(108), 0); // 从下往上进入，进入前应该在外面所以下移故为正，坐标轴是↓→
        }
        &.slide-up-enter-to,&.slide-up-leave {
            transform: translate3d(0, 0, 0);
        }
        &.slide-up-enter-active,&.slide-up-leave-active {
            transition: all 0.3s linear;
        }
        &.hide-box-shadow {
          box-shadow: none;
        }
    }
    .setting-bar {
        z-index: 101;
        width: 100%;
        position: absolute;
        left: 0;
        bottom: px2rem(48);
        height: px2rem(60);
        background-color: white;
        box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, .15);
        &.slide-up-enter,&.slide-up-leave-to {
            transform: translate3d(0, px2rem(108), 0); // 从下往上进入，进入前应该在外面所以下移故为正，坐标轴是↓→
        }
        &.slide-up-enter-to,&.slide-up-leave {
            transform: translate3d(0, 0, 0);
        }
        &.slide-up-enter-active,&.slide-up-leave-active {
            transition: all 0.3s linear;
        }
        .setting-font-size {
            display: flex;
            height: 100%; // 撑开这个设置整个的高度，不然顶到上面去了
            .preview {
                flex: 0 0 px2rem(40);
                @include center;
            }
            .select {
                display: flex; // 整个横条加竖线的集合
                flex: 1; // 如果不设置这个会缩成一块，没有宽度
                .select-wrapper { // 每一个横条加竖线
                    flex: 1;
                    display: flex;
                    align-items: center;
                    &:first-child {
                        .line {
                            &:first-child {
                                border-top: none; // 取消第一个的横线
                            }
                        }
                    }
                    &:last-child {
                        .line {
                            &:last-child {
                                border-top: none; // 取消最后一个的横线
                            }
                        }
                    }
                    .line { // 横线
                        flex: 1;
                        height: 0;
                        border-top: px2rem(1) solid #ccc;
                    }
                    .point-wrapper { // 竖线
                        position: relative;
                        flex: 0 0 0;
                        width: 0;
                        height: px2rem(7);
                        border-left: px2rem(1) solid #ccc;
                        .point { // 小白圆
                            position: absolute;
                            top: px2rem(-8);
                            left: px2rem(-10);
                            width: px2rem(20);
                            height: px2rem(20);
                            border-radius: 50%;
                            background-color: white;
                            border: px2rem(1) solid #ccc;
                            box-shadow: 0 px2rem(4) px2rem(4) rgba(0, 0, 0, .15);
                            @include center;
                            .small-point { // 小黑点
                                width: px2rem(5);
                                height: px2rem(5);
                                background-color: black;
                                border-radius: 50%;
                            }
                        }
                    }
                }
            }
        }
        .setting-theme {
            height: 100%;
            display: flex;
            .setting-theme-item {
                flex: 1;
                display: flex;
                flex-direction: column;
                padding: px2rem(5);
                box-sizing: border-box;
                .preview {
                    flex: 1;
                    border: none;
                    box-sizing: border-box;
                    &.have-border {
                        border: 1px solid #ccc;
                    }
                }
                .text {
                    flex: 0 0 px2rem(20);
                    font-size: px2rem(14);
                    color: #ccc;
                    @include center;
                    &.selected {
                        color: #333;
                    }
                }
            }
        }
    }
}
</style>

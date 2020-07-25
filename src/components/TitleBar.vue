<template>
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
</template>

<script>
export default {
  props: {
    isTitleAndMenuShow: { // 是否显示设置框框
      type: Boolean,
      default: false
    }
  }
}
</script>

<style lang='scss' scoped>
@import '../assets/styles/global';
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
</style>

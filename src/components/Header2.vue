<template>
    <div class="oms2-header-height">
        <header class="oms2-header2">
            <div class="oms2-header-button left">
                <!--<i @click="handleBack" class="fa fa-times oms2-header-handleBack" aria-hidden="true"></i>-->
                <img @click="handleBack" src="./../assets/close_white.png">
            </div>
            <p class="oms2-header-title" v-text="title"></p>
            <div class="oms2-header-button right">
                <slot name="right"></slot>
            </div>
        </header>
    </div>
</template>

<script>
/**
 * oms2-header2
 * @module components/header
 * @desc 顶部导航 返回瑞信
 * @param {string} [title] - 标题
 * @param {slot} [left] - 显示在左侧区域
 * @param {slot} [right] - 显示在右侧区域
 *
 * @example
 * <oms2-header title="我是标题" fixed>
 *   <button slot="left" @click="handleBack">返回</button>
 *   <button slot="right"></button>
 * </oms2-header>
 */
export default {
  name: 'Oms2Header2',
  props: {
    title: String
  },
  mounted() {
    // this.$ruixin.hideWebViewTitle({});
  },
  methods: {
    // 返回按钮
    handleBack() {
      this.$ruixin.closePage({})
      // window.history.length > 1 ? this.$router.go(-1) : this.$ruixin.closePage({});
    }
  }
}
</script>

<style lang="scss">
    .oms2-header-height{
        height: $header-height;
    }
    .oms2-header2 {
        display: flex;
        text-align: center;
        align-items: center;
        background-color: $header-color;
        height: $header-height;
        width: 100%;
        font-size: 20px;
        color: white;
        line-height: 1;
        padding: 0 15px;
        white-space: nowrap;
        position: fixed;
        z-index: 100;
        box-shadow: 2px 2px 3px #e4e4e4;
        .oms2-header-handleBack{
          font-size: 25px;
        }
        .oms2-header-title {
            margin:0px;
            flex: 5;
            color: inherit;
            font-size: inherit;
        }
        .oms2-header-button {
            flex: 1;
            img{
                height:20px;
                width:20px;
            }
        }
        .left {
            text-align: left;
        }
        .right {
            text-align: right;
        }
    }
</style>

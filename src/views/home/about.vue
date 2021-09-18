<!-- home -->
<template>
  <div class="about-container">
    <div class="warpper">
      <div class="list">
        <div class="logo"></div>
        <div class="demo-home__title">VUE H5开发模板</div>
         <div class="item">{{phone | hidePhone}}</div>
         <div class="item">VUE H5开发模板</div>
        <div class="item">VUE H5开发模板</div>
        <div class="item">VUE H5开发模板</div>
        <div class="item">
          {{ userName }}
          <van-button  :loading = "loading" v-if="userName == ''"  loading-text = "提交中..." type="warning" size="small" @click="doDispatch">提交</van-button>
        </div>
        <div class="item">
          <van-button  type="warning" size="small" @click="doCommit">提交</van-button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// 请求接口
import { getUserInfo } from '@/api/user.js'
import { mapGetters } from 'vuex'
import { Toast } from 'vant'
export default {
  data() {
    return {
      phone: '15982387746',
      loading: false
    }
  },
  computed: {
    ...mapGetters(['userName'])
  },
  mounted() {
    this.initData()
  },
  methods: {
    // 请求数据案例
    initData() {
      // 请求接口数据，仅作为展示，需要配置src->config下环境文件
      const params = { }
      getUserInfo(params)
        .then(() => { })
        .catch(() => { })
    },
    // Action 通过 store.dispatch 方法触发
    doDispatch() {
      // Indicator.open()
      this.loading = true
      setTimeout(() => {
        this.$store.dispatch('setUserName', '提交完成')
        this.loading = false
      }, 1000)
    },
    doCommit() {
      // this.$store.commit('showLoading')
      Toast.loading({
        message: '提交中...',
        forbidClick: true,
        loadingType: 'spinner'
      })
      setTimeout(() => {
        Toast.clear()
      }, 5000)
    }
  }
}
</script>
<style lang="scss">
.about-container {
  /* 你的命名空间 */
  background: #fff;
  height: 100vh;
  box-sizing: border-box;
  .warpper {
    padding: 50px 12px 12px 12px;
    .list {
      display: flex;
      flex-direction: column;
      align-items: center;
      color: #666;
      font-size: 14px;
      .demo-home__title {
        margin: 0 0 6px;
        font-size: 32px;
        .demo-home__title img,
        .demo-home__title span {
          display: inline-block;
          vertical-align: middle;
        }
      }
      .item {
        font-size: 14px;
        line-height: 34px;
        a {
          text-decoration: underline;
        }
        .van-button {
          /* vant-ui 元素*/
          background: #ff5500;
        }
      }

      .logo {
        width: 120px;
        height: 120px;
        background: url('./../../assets/logo.png') center / contain no-repeat;
      }
      .wechat {
        width: 200px;
        height: 200px;
        img {
          width: 100%;
          height: auto;
        }
      }
    }
  }
}
</style>

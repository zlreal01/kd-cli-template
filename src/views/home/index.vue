<!-- home -->
<template>
  <div class="index-container">
    <oms2-header :title="$route.meta.title">
    </oms2-header>
    <div class="warpper">
      <h1 class="demo-home__title"><img src="@/assets/logo.png" /><span> VUE H5开发模板</span></h1>
      <h2 class="demo-home__desc">
        A vue h5 template with Vant UI
      </h2>
    </div>
    <van-cell icon="success" v-for="item in list" :key="item" :title="item" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      list: [
        'Vue-cli4',
        'VantUI',
        'Sass',
        'Webpack 4',
        'Vuex ',
        'Axios',
        'Vue-router',
        'ruixin'
      ]
    }
  },

  computed: {},

  mounted() { },

  methods: {}
}
</script>
<style lang="scss" scoped>
.index-container {
  .warpper {
    padding: 12px;
    background: #fff;
    .demo-home__title {
      margin: 0 0 6px;
      font-size: 32px;
      .demo-home__title img,
      .demo-home__title span {
        display: inline-block;
        vertical-align: middle;
      }
      img {
        width: 32px;
      }
      span {
        margin-left: 16px;
        font-weight: 500;
      }
    }
    .demo-home__desc {
      margin: 0 0 20px;
      color: rgba(69, 90, 100, 0.6);
      font-size: 14px;
    }
  }
}
</style>

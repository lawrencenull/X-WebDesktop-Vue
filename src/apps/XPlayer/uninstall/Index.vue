/**
* Created by OXOYO on 2018/4/26.
*
* 自定义卸载界面
*/

<style scoped lang="less" rel="stylesheet/less">
  .app-block {
    width: 100%;
    display: inline-block;

    .block-header {
      .title {
        display: inline-block;
        text-align: center;
        width: 100%;
        padding: 10px;
        margin-bottom: 10px;
        font-size: 20px;
      }
    }
    .block-body {
      display: inline-block;
      text-align: center;
      width: 100%;

      .logo {
        height: 50px;

        img {

        }
      }
      .loading {
        display: inline-block;
        margin-top: 10px;

        .loading-icon{
          animation: loading-spin 1s linear infinite;
        }
        @keyframes loading-spin {
          from { transform: rotate(0deg);}
          50%  { transform: rotate(180deg);}
          to   { transform: rotate(360deg);}
        }
      }
      .complete {
        display: inline-block;
        width: 100%;
        text-align: center;
        margin-top: 10px;
      }
      .button {
        margin-top: 80px;
      }
    }
  }
</style>

<template>
  <div class="app-block">
    <div class="block-header">
      <div class="title">{{ info.app_title || info.config.app.title }}</div>
    </div>
    <div class="block-body">
      <div class="logo">
        <img :src="info.config.app.icon" :alt="info.app_title || info.config.app.title">
      </div>
      <div class="loading" v-show="isLoading">
        <Spin fix>
          <Icon class="loading-icon" type="load-c" size=18></Icon>
          <div class="loading-text">卸载中...</div>
        </Spin>
      </div>
      <div class="complete" v-show="isComplete">{{ isSuccess ? '卸载成功！' : '卸载失败！' }}</div>
      <Button class="button" type="primary" v-show="!isLoading" @click="handleUninstall">立即卸载</Button>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Uninstall',
    props: {
      info: {
        type: Object
      }
    },
    data () {
      return {
        // 是否加载中
        isLoading: false,
        // 是否完成
        isComplete: false,
        // 是否成功
        isSuccess: false
      }
    },
    methods: {
      handleUninstall: function () {
        let _t = this
        _t.isLoading = true
        // 广播事件 触发window事件 open
        _t.$utils.bus.$emit('platform/application/uninstall', {
          action: 'doUninstall',
          data: {
            appInfo: _t.info,
            callback: (isSuccess) => {
              _t.isLoading = false
              _t.isComplete = true
              _t.isSuccess = isSuccess
            }
          }
        })
      }
    }
  }
</script>


<template>
  <div class="loading">
    <img src="./loading.gif" alt="">
    <p class="desc">{{title}}</p>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    props: {
      title: {
        type: String,
        default: '正在载入...'
      }
    }
  }
</script>

<style scoped lang="less" rel="stylesheet/less">
  @import '../../common/css/variable';
  @import '../../common/css/mixin';
  .loading{
    width: 100%;
    text-align: center;
    img{
      width: 24/@num;
      height: 24/@num;
    }
    .desc{
      line-height: 20/@num;
      font-size: @font-size-small;
      color: @color-text-l;
    }
  }
</style>

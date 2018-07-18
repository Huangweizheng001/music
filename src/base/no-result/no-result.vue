<template>
  <div class="no-result">
    <div class="no-result-icon"></div>
    <p class="no-result-text">{{title}}</p>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    props: {
      title: {
        type: String,
        default: ''
      }
    }
  }
</script>

<style scoped lang="less" rel="stylesheet/less">
  @import "../../common/css/variable";
  @import "../../common/css/mixin";
  .no-result{
    text-align: center;
    .no-result-icon{
      width: 86/@num;
      height: 90/@num;
      margin: 0 auto;
      .bg-image('no-result');
      background-size: 86/@num 90/@num;
    }
    .no-result-text{
      margin-top: 30/@num;
      font-size: @font-size-medium;
      color: @color-text-d;
    }
  }
</style>

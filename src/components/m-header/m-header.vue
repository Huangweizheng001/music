<template>
  <div class="m-header">
    <div class="icon"></div>
    <h1 class="text">Chicken Music</h1>
    <router-link to="/user" class="mine" tag="div">
      <i class="icon-mine"></i>
    </router-link>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {}
</script>

<style scoped lang="less" rel="stylesheet/less">
  @import "../../common/css/variable";
  @import "../../common/css/mixin";
  .m-header{
    position: relative;
    height: 44/@num;
    text-align: center;
    color: @color-theme;
    font-size: 0;
    .icon{
      display: inline-block;
      vertical-align: top;
      margin-top: 6/@num;
      width: 30/@num;
      height: 32/@num;
      margin-right: 9/@num;
      .bg-image('logo');
      background-size: 30/@num 32/@num;
    }
    .text{
      display: inline-block;
      vertical-align: top;
      line-height: 44/@num;
      font-size: @font-size-large;
    }
    .mine{
      position: absolute;
      top: 0;
      right: 0;
      .icon-mine{
        display: block;
        padding: 12/@num;
        font-size: 20/@num;
        color: @color-theme;
      }
    }
  }
</style>

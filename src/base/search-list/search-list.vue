<template>
  <div class="search-list" v-show="searches.length">
    <transition-group name="list" tag="ul">
      <!--eslint-disable-next-line-->
      <li @click="selectItem(item)" class="search-item" v-for="item in searches" :key="item">
        <span class="text">{{item}}</span>
        <span class="icon" @click.stop="deleteOne(item)">
          <i class="icon-delete"></i>
        </span>
      </li>
    </transition-group>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    props: {
      searches: {
        type: Array,
        default: []
      }
    },
    methods: {
      selectItem(item) {
        // console.log('嘻嘻')
        this.$emit('select', item)
      },
      deleteOne(item) {
        // console.log('pupu')
        this.$emit('delete', item)
      }
    }
  }
</script>

<style scoped lang="less" rel="stylesheet/less">
  @import "../../common/css/variable";
  @import "../../common/css/mixin";
  .search-list{
    .search-item{
      display: flex;
      align-items: center;
      height: 40/@num;
      overflow: hidden;
      font-size: @font-size-medium;
      &.list-enter-active, &.list-leave-active{
        transition: all 0.1s
      }
      &.list-enter, &.list-leave-to{
        height: 0;
      }
      .text{
        flex: 1;
        color: @color-text-l;
      }
      .icon{
        .extend-click();
        .icon-delete{
          font-size: @font-size-small;
          color: @color-text-d;
        }
      }
    }
  }
</style>

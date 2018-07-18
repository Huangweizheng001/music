<template>
  <div class="song-list">
    <ul>
      <!--eslint-disable-next-line-->
      <li v-for="(song,index) in songs" class="item" @click="selectItem(song,index)">
        <div class="rank" v-show="rank">
          <span :class="getRankCls(index)">{{getRankText(index)}}</span>
        </div>
        <div class="content">
          <h2 class="name">{{song.name}}</h2>
          <p class="desc">{{getDesc(song)}}</p>
        </div>
      </li>
    </ul>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    props: {
      songs: {
        type: Array,
        default: null
      },
      rank: {
        type: Boolean,
        default: false
      }
    },
    methods: {
      selectItem(item, index) {
        this.$emit('select', item, index)
      },
      getDesc(song) {
        return `${song.singer} 。${song.album}`
      },
      getRankCls(index) {
        if (index <= 2) {
          return `icon icon${index}`
        } else {
          return 'text'
        }
      },
      getRankText(index) {
        if (index > 2) {
          return index + 1
        }
      }
    }
  }
</script>

<style scoped lang="less" rel="stylesheet/less">
  @import "../../common/css/mixin";
  @import "../../common/css/variable";
  .song-list{
    .item{
      display: flex;
      align-items: center;
      box-sizing: border-box;
      height: 64/@num;
      font-size: @font-size-medium;
      .rank{
        flex: 0 0 25/@num;
        width: 25/@num;
        margin-right: 30/@num;
        text-align: center;
        .icon{
          display: inline-block;
          width: 25/@num;
          height: 24/@num;
          background-size: 25/@num 24/@num;
          &.icon0{
            .bg-image('first');
          }
          &.icon1{
            .bg-image('second')
          }
          &.icon2{
            .bg-image('third')
          }
        }
        .text{
          color: @color-theme;
          font-size: @font-size-large;
        }
      }
      .content{
        flex: 1;
        line-height: 20/@num;
        overflow: hidden;
        .name{
          .no-wrap();
          color: @color-text;
        }
        .desc{
          .no-wrap();
          margin-top: 4/@num;
          color: @color-text-d;
        }
      }
    }
  }
</style>

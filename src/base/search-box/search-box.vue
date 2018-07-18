<template>
  <div class="search-box">
    <i class="icon-search"></i>
    <input class="box" :placeholder="placeholder" v-model="query" ref="query">
    <i v-show="query" class="icon-dismiss" @click="clear"></i>
  </div>
</template>

<script type="text/ecmascript-6">
  import {debounce} from 'common/js/util'

  export default {
    props: {
      placeholder: {
        type: String,
        default: '搜索歌曲、歌手'
      }
    },
    data() {
      return {
        query: ''
      }
    },
    methods: {
      clear() {
        this.query = ''
      },
      setQuery(query) {
        this.query = query
      },
      blur() {
        this.$refs.query.blur()
      }
    },
    created() {
      this.$watch('query', debounce((newQuery) => {
        this.$emit('query', newQuery)
      }, 200))
    }
  }
</script>

<style scoped lang="less" rel="stylesheet/less">
  @import "../../common/css/variable";
  @import "../../common/css/mixin";
  .search-box{
    display: flex;
    align-items: center;
    box-sizing: border-box;
    width: 100%;
    padding: 0 6/@num;
    height: 40/@num;
    background: @color-highlight-background;
    border-radius: 6px;
    .icon-search{
      font-size: 24/@num;
      color: @color-background;
    }
    .box{
      flex: 1;
      margin: 0 5/@num;
      line-height: 18/@num;
      background: @color-highlight-background;
      color: @color-text;
      font-size: @font-size-medium;
      outline: 0;
      &::placeholder{
        color: @color-text-d
      }
    }
    .icon-dismiss{
      font-size: 16/@num;
      color: @color-background;
    }
  }
</style>

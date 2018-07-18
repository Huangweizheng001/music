<template>
  <div class="rank" ref="rank">
    <scroll class="toplist" :data="topList" ref="scroll">
      <ul>
        <!--eslint-disable-next-line-->
        <li class="item" v-for="item in topList" @click="selectItem(item)">
          <div class="icon">
            <img v-lazy="item.picUrl">
          </div>
          <ul class="songlist">
            <!--eslint-disable-next-line-->
            <li class="song" v-for="(song,index) in item.songList">
              <span>{{index + 1}}</span>
              <span>{{song.songname}}-{{song.singername}}</span>
            </li>
          </ul>
        </li>
      </ul>
      <div class="loading-container" v-show="!topList.length">
        <loading></loading>
      </div>
    </scroll>
    <router-view></router-view>
  </div>
</template>

<script type="text/ecmascript-6">
  import {getTopList} from 'api/rank'
  import Scroll from 'base/scroll/scroll'
  import {ERR_OK} from 'api/config'
  import Loading from 'base/loading/loading'
  import {playlistMixin} from 'common/js/mixin'
  import {mapMutations} from 'vuex'

  let htmlWidth = document.documentElement.clientWidth || document.body.clientWidth
  const BOTTOM_HEIGHT = 60 / 37.5 * (htmlWidth / 10)
  export default {
    mixins: [playlistMixin],
    created() {
      this._getTopList()
    },
    data() {
      return {
        topList: []
      }
    },
    methods: {
      handlePlaylist(playlist) {
        const bottom = playlist.length > 0 ? BOTTOM_HEIGHT : ''
        this.$refs.rank.style.bottom = `${bottom}px`
        this.$refs.scroll.refresh()
      },
      selectItem(item) {
        this.$router.push({
          path: `/rank/${item.id}`
        })
        this.setTopList(item)
      },
      _getTopList() {
        getTopList().then((res) => {
          console.log(res)
          if (res.code === ERR_OK) {
            this.topList = res.data.topList
          }
        })
      },
      ...mapMutations({
        setTopList: 'SET_TOP_LIST'
      })
    },
    components: {
      Scroll,
      Loading
    }
  }
</script>

<style scoped lang="less" rel="stylesheet/less">
  @import "../../common/css/variable";
  @import "../../common/css/mixin";
  .rank{
    position: fixed;
    width: 100%;
    top: 88/@num;
    bottom: 0;
    .toplist{
      height: 100%;
      overflow: hidden;
      .item{
        display: flex;
        margin: 0 20/@num;
        padding-top: 20/@num;
        height: 100/@num;
        &:last-child{
          padding-bottom: 20/@num
        }
        .icon{
          flex: 0 0 100/@num;
          width: 100/@num;
          height: 100/@num;
          img{
            width: 100/@num;
            height: 100/@num;
          }
        }
        .songlist{
          flex: 1;
          display: flex;
          flex-direction: column;
          justify-content: center;
          padding: 0 20/@num;
          height: 100/@num;
          overflow: hidden;
          background: @color-highlight-background;
          color: @color-text-d;
          font-size: @font-size-small;
          .song{
            .no-wrap();
            line-height: 26/@num;
          }
        }
      }
      .loading-container{
        position: absolute;
        width: 100%;
        top: 50%;
        transform: translateY(-50%);
      }
    }
  }
</style>

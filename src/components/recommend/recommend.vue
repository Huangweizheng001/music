<template>
  <div class="recommend" ref="recommend">
    <scroll class="recommend-content" :data="discList" ref="scroll">
      <div>
        <div class="slider-wrapper" v-if="recommends.length">
          <slider>
            <!--eslint-disable-next-line-->
            <div v-for="item in recommends">
              <a :href="item.linkUrl">
                <img @load="loadImage" :src="item.picUrl" alt="" class="needsclick">
              </a>
            </div>
          </slider>
        </div>
        <div class="recommend-list">
          <h1 class="list-title">热门歌单推荐</h1>
          <ul>
            <!--eslint-disable-next-line-->
            <li @click="selectItem(item)" v-for="item in discList" class="item">
              <div class="icon">
                <img v-lazy="item.imgurl" alt="">
              </div>
              <div class="text">
                <h2 class="name" v-html="item.creator.name"></h2>
                <p class="desc" v-html="item.dissname"></p>
              </div>
            </li>
          </ul>
        </div>
      </div>
      <div class="loading-container" v-show="!discList.length">
        <loading></loading>
      </div>
    </scroll>
    <router-view></router-view>
  </div>
</template>

<script type="text/ecmascript-6">
  import Loading from 'base/loading/loading'
  import Scroll from 'base/scroll/scroll'
  import Slider from 'base/slider/slider'
  import {getRecommend, getDiscList} from 'api/recommend'
  import {ERR_OK} from 'api/config'
  import {playlistMixin} from 'common/js/mixin'
  import {mapMutations} from 'vuex'

  let htmlWidth = document.documentElement.clientWidth || document.body.clientWidth
  const BOTTOM_HEIGHT = 60 / 37.5 * (htmlWidth / 10)
  export default {
    mixins: [playlistMixin],
    data() {
      return {
        recommends: [],
        discList: []
      }
    },
    created() {
      setTimeout(() => {
        this._getRecommend()
        this._getDiscList()
      }, 20)
    },
    methods: {
      handlePlaylist(playlist) {
        const bottom = playlist.length > 0 ? BOTTOM_HEIGHT : ''
        this.$refs.recommend.style.bottom = `${bottom}px`
        this.$refs.scroll.refresh()
      },
      selectItem(item) {
        this.$router.push({
          path: `/recommend/${item.dissid}`
        })
        this.setDisc(item)
      },
      _getRecommend() {
        getRecommend().then((res) => {
          if (res.code === ERR_OK) {
            this.recommends = res.data.slider
            // console.log(res.data.slider)
          }
        })
      },
      _getDiscList() {
        getDiscList().then((res) => {
          if (res.code === ERR_OK) {
            this.discList = res.data.list
            // console.log(res.data.list)
          }
        })
      },
      loadImage() {
        if (!this.checkLoaded) {
          this.$refs.scroll.refresh()
          this.checkLoaded = true
        }
      },
      ...mapMutations({
        setDisc: 'SET_DISC'
      })
    },
    components: {
      Slider,
      Scroll,
      Loading
    }
  }
</script>

<style scoped lang="less" rel="stylesheet/less">
  @import "../../common/css/variable";
  @import "../../common/css/mixin";
  .recommend{
    position: fixed;
    width: 100%;
    top: 88/@num;
    bottom: 0;
    .recommend-content{
      height: 100%;
      overflow: hidden;
      .slider-wrapper{
        position: relative;
        width: 100%;
        overflow: hidden;
      }
      .recommend-list{
        .list-title{
          height: 65/@num;
          line-height: 65/@num;
          text-align: center;
          font-size: @font-size-medium;
          color: @color-theme;
        }
        .item{
          display: flex;
          box-sizing: border-box;
          align-items: center;
          padding: 0 20/@num 20/@num 20/@num;
          .icon{
            flex: 0 0 60/@num;
            width: 60/@num;
            padding-right: 20/@num;
            img{
              width: 60/@num;
              height: 60/@num;
            }
          }
          .text{
            display: flex;
            flex-direction: column;
            justify-content: center;
            flex: 1;
            line-height: 20/@num;
            overflow: hidden;
            font-size: @font-size-medium;
            .name{
              margin-bottom: 10/@num;
              color: @color-text;
            }
            .desc{
              color: @color-text-d;
            }
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

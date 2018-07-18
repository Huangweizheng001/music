<template>
  <transition name="list-fade">
    <div class="playlist" v-show="showFlag" @click="hide">
      <div class="list-wrapper" @click.stop>
        <div class="list-header">
          <h1 class="title">
            <i class="icon" :class="iconMode" @click="changeMode"></i>
            <span class="text">{{modeText}}</span>
            <span class="clear" @click="showConfirm"><i class="icon-clear"></i></span>
          </h1>
        </div>
        <Scroll class="list-content" :data="sequenceList" ref="listContent" :refreshDelay="refreshDelay">
          <transition-group name="list" tag="ul">
            <!--eslint-disable-next-line-->
            <li :key="item.id" class="item" v-for="(item,index) in sequenceList" @click="selectItem(item,index)" ref="listItem">
              <i class="current" :class="getCurrentIcon(item)"></i>
              <span class="text">{{item.name}}</span>
              <span @click.stop="toggleFavorite(item)" class="like">
                <i :class="getFavoriteIcon(item)"></i>
              </span>
              <span class="delete" @click.stop="deleteOne(item)">
                <i class="icon-delete"></i>
              </span>
            </li>
          </transition-group>
        </Scroll>
        <div class="list-operate">
          <div class="add" @click="addSong">
            <i class="icon-add"></i>
            <span class="text">添加歌曲到队列</span>
          </div>
        </div>
        <div class="list-close" @click="hide">
          <span>关闭</span>
        </div>
      </div>
      <confirm ref="confirm" text="是否清空播放列表" confirmBtnText="清空" @confirm="confirmClear"></confirm>
      <add-song ref="addSong"></add-song>
    </div>
  </transition>
</template>

<script type="text/ecmascript-6">
  import {mapActions} from 'vuex'
  import {playMode} from 'common/js/config'
  import Scroll from 'base/scroll/scroll'
  import Confirm from 'base/confirm/confirm'
  import {playerMixin} from 'common/js/mixin'
  import AddSong from 'components/add-song/add-song'

  export default {
    mixins: [playerMixin],
    data() {
      return {
        showFlag: false,
        refreshDelay: 100
      }
    },
    computed: {
      modeText() {
        return this.mode === playMode.sequence ? '顺序播放' : this.mode === playMode.random ? '随机播放' : '单曲循环'
      }
    },
    methods: {
      show() {
        this.showFlag = true
        setTimeout(() => {
          this.$refs.listContent.refresh()
          this.scrollToCurrent(this.currentSong)
        }, 20)
      },
      hide() {
        this.showFlag = false
      },
      getCurrentIcon(item) {
        if (this.currentSong.id === item.id) {
          return 'icon-play'
        }
        return ''
      },
      selectItem(item, index) {
        if (this.mode === playMode.random) {
          index = this.playlist.findIndex((song) => {
            return song.id === item.id
          })
        }
        this.setCurrentIndex(index)
        this.setPlayingState(true)
      },
      scrollToCurrent(current) {
        const index = this.sequenceList.findIndex((song) => {
          return current.id === song.id
        })
        // console.log(index)
        this.$refs.listContent.scrollToElement(this.$refs.listItem[index], 300)
      },
      deleteOne(item) {
        this.deleteSong(item)
        if (!this.playlist.length) {
          this.hide()
        }
      },
      confirmClear() {
        this.deleteSongList()
        this.$refs.confirm.hide()
      },
      showConfirm() {
        this.$refs.confirm.show()
      },
      addSong() {
        this.$refs.addSong.show()
      },
      ...mapActions([
        'deleteSong',
        'deleteSongList'
      ])
    },
    watch: {
      currentSong(newSong, oldSong) {
        if (!this.showFlag || newSong.id === oldSong.id) {
          return
        }
        console.log('哈哈哈')
        this.scrollToCurrent(newSong)
      }
    },
    components: {
      Scroll,
      Confirm,
      AddSong
    }
  }
</script>

<style scoped lang="less" rel="stylesheet/less">
  @import "../../common/css/variable";
  @import "../../common/css/mixin";
  .playlist{
    position: fixed;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    z-index: 200;
    background-color: @color-background-d;
    &.list-fade-enter-active, &.list-fade-leave-active{
      transition: opacity 0.3s;
      .list-wrapper{
        transition: all 0.3s;
      }
    }
    &.list-fade-enter, &.list-fade-leave-to{
      opacity: 0;
      .list-wrapper{
        transform: translate3d(0, 100%, 0);
      }
    }
    &.list-fade-enter{

    }
    .list-wrapper{
      position: absolute;
      left: 0;
      bottom: 0;
      width: 100%;
      background-color: @color-highlight-background;
      .list-header{
        position: relative;
        padding: 20/@num 30/@num 10/@num 20/@num;
        .title{
          display: flex;
          align-items: center;
          .icon{
            margin-right: 10/@num;
            font-size: 30/@num;
            color: @color-theme-d;
          }
          .text{
            flex: 1;
            font-size: @font-size-medium;
            color: @color-text-l;
          }
          .clear{
            .extend-click();
            .icon-clear{
              font-size: @font-size-medium;
              color: @color-text-d;
            }
          }
        }
      }
      .list-content{
        max-height: 240/@num;
        overflow: hidden;
        .item{
          display: flex;
          align-items: center;
          height: 40/@num;
          padding: 0 30/@num 0 20/@num;
          overflow: hidden;
          &.list-enter-active, &.list-leave-active{
            transition: all 0.1s
          }
          &.list-enter, &.list-leave-to{
            height: 0
          }
          .current{
            flex: 0 0 20/@num;
            width: 20/@num;
            font-size: @font-size-small;
            color: @color-theme-d;
          }
          .text{
            flex: 1;
            .no-wrap();
            font-size: @font-size-medium;
            color: @color-text-d;
          }
          .like{
            .extend-click();
            margin-right: 15/@num;
            font-size: @font-size-small;
            color: @color-theme;
            .icon-favorite{
              color: @color-sub-theme;
            }
          }
          .delete{
            .extend-click();
            font-size: @font-size-small;
            color: @color-theme;
          }
        }
      }
      .list-operate{
        width: 140/@num;
        margin: 20/@num auto 30/@num auto;
        .add{
          display: flex;
          align-items: center;
          padding: 8/@num 16/@num;
          border: 1px solid @color-text-l;
          border-radius: 100/@num;
          color: @color-text-l;
          .icon-add{
            margin-right: 5/@num;
            font-size: @font-size-small-s;
          }
          .text{
            font-size: @font-size-small
          }
        }
      }
      .list-close{
        text-align: center;
        line-height: 50/@num;
        background: @color-background;
        font-size: @font-size-medium-x;
        color: @color-text-l;
      }
    }
  }
</style>

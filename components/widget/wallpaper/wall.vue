<template>
  <div id="wallpaper-wall">
    <div class="wall-box">
      <transition name="module" mode="out-in">
        <div
          class="picture-box"
          ref="picture-box"
          :title="currentWallpaper.copyright"
          :key="currentWallpaperUrl"
          :style="{
            backgroundImage: `url(${currentWallpaperUrl})`
          }"
        />
      </transition>
      <div class="story-box">
        <div class="empty" v-if="!currentWallpaper">挂了</div>
        <div class="content" v-else>
          <h2 class="title">{{ currentWallpaper.title }}</h2>
          <p class="sub-title">{{ currentWallpaper.copyright }}</p>
          <p class="desc">{{ currentWallpaper.desc }}</p>
          <div class="tools">
            <transition name="module">
              <span class="location" key="location">
                <i class="iconfont icon-location"></i>
                <span>{{ currentWallpaper.bsTitle }}</span>
              </span>
            </transition>
            <transition name="module">
              <a
                class="link"
                key="link"
                target="_blank"
                rel="external nofollow noopenter"
                :href="currentWallpaper.searchUrl"
              >
                <i class="iconfont icon-link"></i>
                <span>相关链接</span>
              </a>
            </transition>
            <button class="btn" title="上一幅" :disabled="!canNext" @click="index++">
              <i class="iconfont icon-prev"></i>
            </button>
            <button class="btn" title="下一幅" :disabled="!canPrev" @click="index--">
              <i class="iconfont icon-next"></i>
            </button>
            <button class="btn" title="春尽江南" @click="close">
              <i class="iconfont icon-cancel"></i>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'wallpaper-wall',
    data() {
      return {
        index: 0
      }
    },
    methods: {
      close() {
        this.$store.commit('global/updateWallpaperOnState', false)
      }
    },
    computed: {
      wallpapers() {
        return this.$store.state.wallpaper.papers.data
      },
      currentWallpaper() {
        return this.wallpapers && this.wallpapers.length && this.wallpapers[this.index]
      },
      currentWallpaperUrl() {
        return this.currentWallpaper ? this.currentWallpaper.humanizeUrl : ''
      },
      canPrev() {
        return this.index > 0
      },
      canNext()  {
        return this.wallpapers ? (this.index < this.wallpapers.length - 1) : false
      }
    }
  }
</script>

<style lang="scss" scoped>
  #wallpaper-wall {
    position: fixed;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100vw;
    height: 100vh;
    top: 0;
    left: 0;
    z-index: 1000;
    background-color: $module-hover-bg-darken-20;

    > .wall-box {
      position: relative;
      overflow: auto;
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      align-items: center;
      width: calc(100vw * 0.88);
      height: calc(100vh * 0.88);
      background-color: $module-bg;
      border: solid .5rem $module-hover-bg;

      > .picture-box {
        width: 100%;
        height: 100%;
        overflow: hidden;
        background-repeat: no-repeat;
        background-size: cover;
      }

      > .story-box {
        position: absolute;
        padding: 2rem 4rem;
        background-color: $module-hover-bg;
        bottom: 0;
        width: 100%;
        height: auto;

        > .content {
          color: $white;

          > .title {
            margin-top: 0;
          }

          > .desc {
            line-height: 2rem;
          }

          > .tools {
            color: $text;

            > .link {
              text-decoration: none;

              > span {
                position: relative;

                &:after {
                  content: '';
                  position: absolute;
                  bottom: -2px;
                  left: 0;
                  width: 100%;
                  height: 0.5px;
                  background-color: $text;
                }
              }
            }

            > .location,
            > .link,
            > .btn {
              display: block;
              float: left;
              background-color: $module-bg;
              margin-right: 1rem;
              height: 3rem;
              line-height: 3rem;
              padding: 0 1rem;
            }

            > .link,
            > .btn {
              &:hover {
                background-color: $module-hover-bg-opacity-9;
              }
            }

            > .btn {

              &[disabled] {
                opacity: .6;
              }
            }
          }
        }
      }
    }
  }
</style>

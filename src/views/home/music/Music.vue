<template>
  <div class="music">
    <slider>
      <swiper-slide class="slider" v-for="banner in banners" :key="banner.encodeId">
        <a :href="banner.url">
          <img :src="banner.pic">
        </a>
      </swiper-slide>
    </slider>
    <ul class="tab">
      <li class="tab-item">
        <router-link to="fm" class="link">
          <div class="container">
            <i class="iconfont icon-iconkpxd"></i>
          </div>
          <p class="name">私人FM</p>
        </router-link>
      </li>
      <li class="tab-item">
        <router-link to="fm" class="link">
          <div class="container">
            <i class="iconfont icon-calendar"></i>
          </div>
          <p class="name">每日推荐</p>
        </router-link>
      </li>
      <li class="tab-item">
        <router-link to="/home/music/songList" class="link">
          <div class="container">
            <i class="iconfont icon-liebiao"></i>
          </div>
          <p class="name">歌单</p>
        </router-link>
      </li>
      <li class="tab-item">
        <router-link to="fm" class="link">
          <div class="container">
            <i class="iconfont icon-paixingbang"></i>
          </div>
          <p class="name">排行榜</p>
        </router-link>
      </li>
    </ul>
    <recommend-song-list :items="recommendSongs"></recommend-song-list>
    <du-jia-fang-song :data="personalRecommends"></du-jia-fang-song>
    <latest-music :data="latestMusic"></latest-music>
    <transition name="slide">
      <keep-alive>
        <router-view></router-view>
      </keep-alive>
    </transition>
  </div>
</template>
<script>
import Slider from '@/components/Slider'
import RecommendSongList from './RecommendSongList'
import DuJiaFangSong from './DuJiaFangSong'
import LatestMusic from './LatestMusic'
import MusicService from '@/api/music'
export default {
  data () {
    return {
      banners: [],
      recommendSongs: [],
      personalRecommends: [],
      latestMusic: []
    }
  },
  created () {
    this.getBanner()
    this.getRecommendSongs()
    this.getPersonalRecommends()
    this.getLatestMusic()
  },
  methods: {
    getBanner () {
      MusicService.banner().then(res => {
        this.banners = res ? res.banners : []
      })
    },
    getRecommendSongs () {
      MusicService.getRecommendSongs().then(res => {
        let songs = res ? (res.result ? res.result : []) : []
        if (songs.length > 6) {
          songs = songs.slice(0, 6)
        }
        this.recommendSongs = songs
      })
    },
    getPersonalRecommends () {
      MusicService.getPersonalRecommends().then(res => {
        let songs = res ? (res.result ? res.result : []) : []
        this.personalRecommends = songs
      })
    },
    getLatestMusic () {
      MusicService.getLatestMusic(0, 6).then(res => {
        if (res.code === 200) {
          let songs = res ? (res.albums ? res.albums : []) : []
          this.latestMusic = songs
        }
      })
    }
  },
  components: {
    Slider,
    RecommendSongList,
    DuJiaFangSong,
    LatestMusic
  }
}
</script>

<style lang="scss" scoped>
@import '../../../assets/style/var.scss';
.slider {
  a {
    width: 100%;
    display: block;
  }
  img {
    width: 100%;
    display: block;
  }
}
.tab {
  background: $color-background;
  display: flex;
  justify-content: space-around;
  align-items: center;
  border-bottom: rem(2) solid $color-border;
  border-top: rem(2) solid $color-border;
  .tab-item {
    height: rem(210);
    a {
      display: flex;
      height: 100%;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      .container {
        width: rem(100);
        height: rem(100);
        border: rem(2) solid $color-theme-s;
        border-radius: rem(50);
        display: flex;
        justify-content: center;
        align-items: center;
        .iconfont {
          color: $color-theme-s;
          @include font(26);
        }
      }
      .name {
        @include font(12);
        color: $color-text;
        padding-top: rem(20);
      }
    }
  }
}
</style>

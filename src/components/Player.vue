<template>
  <section class="container">
    <div class="player">
      <img :src="album.cover" alt="" />
      <h3 class="song-title">
        {{ current.name }}
      </h3>
      <span>{{ current.artist }}</span>
    </div>
    <div class="progress" ref="progress">
      <div class="progress__top">
        <div class="progress__time">{{ currentTime }}</div>
        <div class="progress__duration">{{ duration }}</div>
      </div>
      <div class="progress__bar" @click="clickProgress">
        <div class="progress__current" :style="{ width: barWidth }"></div>
      </div>
    </div>
    <div class="control">
      <button class="prev" @click="prev">
        <font-awesome-icon :icon="['fas', 'backward']" />
      </button>
      <button class="play" v-if="!isPlaying" @click="play">
        <font-awesome-icon :icon="['fas', 'play']" />
      </button>
      <button class="pause" v-else @click="pause">
        <font-awesome-icon :icon="['fas', 'pause']" />
      </button>
      <button class="next" @click="next">
        <font-awesome-icon :icon="['fas', 'forward']" />
      </button>
    </div>
  </section>
</template>

<script>
export default {
  name: 'player',
  props: ['album'],
  watch: {
    album: function() {
      this.current = this.album.tracks[this.index];
      this.resetPlayer();
    },
  },
  data() {
    return {
      // new
      currentTrack: null,
      duration: null,
      currentTime: null,
      barWidth: null,
      current: {},
      isPlaying: false,
      index: 0,
      coverImage: '',
      playlist: [
        {
          title: '01 - 墜落',
          artist: '賀爾蒙少年',
          src: require('../assets/黑色台北(Urban Noir)/01 - 墜落.mp3'),
        },
        {
          title: '02 - 中華商場1971 feat.修齊',
          artist: '賀爾蒙少年',
          src: require('../assets/黑色台北(Urban Noir)/02 - 中華商場1971 feat.修齊.mp3'),
        },
        {
          title: '03 - 遙遠的星星',
          artist: '賀爾蒙少年',
          src: require('../assets/黑色台北(Urban Noir)/03 - 遙遠的星星.mp3'),
        },
      ],
      player: new Audio(),
    };
  },
  created() {
    let vm = this;
    this.current = this.album.tracks[this.index];
    this.player.src = this.current.src;
    this.player.ontimeupdate = function() {
      vm.generateTime();
    };
    this.player.onloadedmetadata = function() {
      vm.generateTime();
    };
  },
  beforeDestroy() {
    this.pause();
    this.resetPlayer();
  },
  methods: {
    play(song) {
      if (typeof song.src != 'undefined') {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    next() {
      this.index++;
      if (this.index > this.album.tracks.length - 1) {
        this.index = 0;
      }
      this.current = this.album.tracks[this.index];
      this.resetPlayer();
    },
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.album.tracks.length - 1;
      }
      this.current = this.album.tracks[this.index];
      this.resetPlayer();
    },
    resetPlayer() {
      this.barWidth = 0;
      this.circleLeft = 0;
      this.player.currentTime = 0;
      this.player.src = this.current.src;
      setTimeout(() => {
        if (this.isPlaying) {
          this.player.play();
        } else {
          this.player.pause();
        }
      }, 300);
    },
    updateBar(x) {
      let progress = this.$refs.progress;
      let maxduration = this.player.duration;
      let position = x - progress.offsetLeft;
      let percentage = (100 * position) / progress.offsetWidth;
      if (percentage > 100) {
        percentage = 100;
      }
      if (percentage < 0) {
        percentage = 0;
      }
      this.barWidth = percentage + '%';
      this.circleLeft = percentage + '%';
      this.player.currentTime = (maxduration * percentage) / 100;
      this.player.play();
    },
    clickProgress(e) {
      this.isPlaying = true;
      this.player.pause();
      this.updateBar(e.pageX);
    },
    generateTime() {
      let width = (100 / this.player.duration) * this.player.currentTime;
      this.barWidth = width + '%';
      this.circleLeft = width + '%';
      let durmin = Math.floor(this.player.duration / 60);
      let dursec = Math.floor(this.player.duration - durmin * 60);
      let curmin = Math.floor(this.player.currentTime / 60);
      let cursec = Math.floor(this.player.currentTime - curmin * 60);
      if (durmin < 10) {
        durmin = '0' + durmin;
      }
      if (dursec < 10) {
        dursec = '0' + dursec;
      }
      if (curmin < 10) {
        curmin = '0' + curmin;
      }
      if (cursec < 10) {
        cursec = '0' + cursec;
      }
      this.duration = durmin + ':' + dursec;
      this.currentTime = curmin + ':' + cursec;
    },
  },
};
</script>

<style lang="scss">
.container {
  margin-top: 80px;
  width: 100%;
}
.player {
  display: flex;
  flex-direction: column;
  align-items: center;

  img {
    width: 200px;
    box-shadow: 0px 8px 20px 0px gray;
  }
}
.album-info {
  color: #71829e;
  flex: 1;
  padding-right: 60px;
  user-select: none;

  @media screen and (max-width: 576px), (max-height: 500px) {
    padding-right: 30px;
  }

  &__name {
    font-size: 20px;
    font-weight: bold;
    margin-bottom: 12px;
    line-height: 1.3em;
    @media screen and (max-width: 576px), (max-height: 500px) {
      font-size: 18px;
      margin-bottom: 9px;
    }
  }
  &__track {
    font-weight: 400;
    font-size: 20px;
    opacity: 0.7;
    line-height: 1.3em;
    min-height: 52px;
    @media screen and (max-width: 576px), (max-height: 500px) {
      font-size: 18px;
      min-height: 50px;
    }
  }
}
.progress {
  width: 100%;
  margin-top: -15px;
  user-select: none;
  &__top {
    margin: 0px 30px;
    display: flex;
    align-items: flex-end;
    justify-content: space-between;
  }

  &__duration {
    color: #71829e;
    font-weight: 400;
    font-size: 12px;
    opacity: 0.5;
  }
  &__time {
    margin-top: 2px;
    color: #71829e;
    font-weight: 400;
    font-size: 12px;
    opacity: 0.7;
  }
}
.progress__bar {
  height: 3px;
  width: 100%;
  cursor: pointer;
  background-color: white;
  display: inline-block;
  border-radius: 10px;
}
.progress__current {
  height: inherit;
  width: 0%;
  background-color: #f9179a;
  border-radius: 10px;
}
</style>

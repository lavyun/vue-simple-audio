<template>
  <div class="player"
       :style="{'background-color': bgColor,'transform':'scale(' + (parseInt(width)/190) +')'}">
    <div class="player-panel">
      <div class="player-panel-details">
        <div class="player-panel-title text-ellipsis" v-cloak :title="currentAudio.songname">
          {{currentAudio.songname}}
        </div>
        <div class="player-panel-time">
          <span class="player-time-current">{{ current | time }}</span>
          <span class="player-time-total">of {{ duration | time }}</span>
        </div>
      </div>
      <div class="player-panel-volume" @click="changeVolume($event)" ref="volume-control">
        <i class="player-volume-control" :volume-level="(6-index)*2" v-for="index in 5"
           :class="{'backBlack': (volume/2 - 5 + index > 0) ? true : false }"></i>
        <i class="icon-headphones" @click="noVolume()"></i>
        <i class="no-volume" v-show="!volume"></i>
      </div>
    </div>
    <div class="player-controls">
      <div class="player-buttons">
        <div class="player-btn">
          <span class="player-prev" @click="prev()"></span>
        </div>
        <div class="player-btn">
          <span class="player-pause" @click="toggleStatus()" :class="{'player-play':isPause}"></span>
        </div>
        <div class="player-btn">
          <span class="player-next" @click="next()"></span>
        </div>
      </div>
      <div style="clear: both"></div>
      <div class="player-range">
        <mt-range v-model="current" :min="0" :max="duration" :step="1"
                  @mousedown.native="changeRange($event)">
        </mt-range>
      </div>
    </div>
    <audio :src="currentAudio.url" id="player" :autoplay="autoPlay" :loop="loop" @timeupdate="timeChange()"
           @loadeddata="getDuration()" @ended="next()"
           ref="player" @error="next()">
    </audio>
  </div>
</template>

<script type="es6">
  import Range from '../lib/mint-range';
  import './style.css';

  export default {
    props: {
      autoPlay: {
        type: Boolean,
        default: true
      },
      repeat: {
        type: Boolean,
        default: true
      },
      loop: {
        type: Boolean,
        default: false
      },
      width: {
        type: String,
        default: '300'
      },
      initialVolume: {
        type: String,
        default: '60'
      },
      bgColor: {
        type: String,
        default: '#000000'
      },
      songs: {
        type: Array
      }
    },
    data: function () {
      return {
        RANGE_WIDTH: 180,
        current: 0,
        duration: 0,
        oldVolume: 0,
        audioIndex: 0,
        volume: parseInt(this.initialVolume) / 10,
        isPause: !this.autoPlay
      }
    },
    components:{
      'mt-range':Range
    },
    mounted: function () {
      this.$refs.player.volume = this.volume / 10;
    },
    computed: {
      currentAudio: function () {
        return this.songs[this.audioIndex];
      }
    },
    methods: {
      toggleStatus: function () {
        var player = this.$refs.player;
        this.isPause ? player.play() : player.pause();
        this.isPause = !this.isPause;
      },
      timeChange: function () {
        this.current = this.$refs.player.currentTime;
      },
      getDuration: function () {
        this.duration = this.$refs.player.duration;
      },
      changeRange: function (event) {
        var offset = event.offsetX;
        if (this.duration && offset < this.RANGE_WIDTH) {
          var currentClick = Math.floor(offset * this.duration / this.RANGE_WIDTH);
          this.$refs.player.currentTime = currentClick;
        }
      },
      changeVolume: function (event) {
        if (event.target.classList.contains('player-volume-control')) {
          this.volume = parseInt(event.target.getAttribute('volume-level'));
          this.$refs.player.volume = this.volume / 10;
        }
      },
      noVolume: function () {
        if (this.volume) {
          this.oldVolume = this.volume;
          this.volume = 0;
        } else {
          this.volume = this.oldVolume;
        }
        this.$refs.player.volume = this.volume / 10;
      },
      prev: function () {
        if (this.audioIndex == 0) {
          if (this.repeat) {
            this.audioIndex = this.songs.length - 1;
          }
        } else {
          this.audioIndex--;
        }
      },
      next: function () {
        if (this.audioIndex == this.songs.length - 1) {
          if (this.repeat) {
            this.audioIndex = 0;
          }
        } else {
          this.audioIndex++;
        }
      }
    },
    filters: {
      time: function (value) {
        var length = Math.floor(parseInt(value));
        var minute = Math.floor(value / 60);
        if (minute < 10) {
          minute = '0' + minute;
        }
        var second = length % 60;
        if (second < 10) {
          second = '0' + second;
        }
        return minute + ':' + second;
      }
    }
  }
</script>


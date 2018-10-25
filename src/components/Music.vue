<template>
  <div class="icon rotate " :class="{audio:!audio.playing}" id="audio_btn" @click="startPlayOrPause">
      <audio
        ref="audio"
        preload="auto"
        loop=""
        class="audio1"
        id="myAudio"
        @pause="onPause"
        @play="onPlay">
          <source :src="url" type="audio/mpeg">
      </audio>
  </div>
</template>
<script>
export default {
  name: "Music",
  props: ["musicsrc"],
  data() {
    return {
      url: this.musicsrc || "http://p68hrlf4v.bkt.clouddn.com/qwt.mp3",
      audio: {
        currentTime: 0,
        maxTime: 0,
        playing: false,
        muted: false,
        speed: 1,
        waiting: true,
        preload: "auto"
      },

      sliderTime: 0,
      volume: 100,
      speeds: this.theSpeeds,

      controlList: {
        // 不显示下载
        noDownload: false,
        // 不显示静音
        noMuted: false,
        // 不显示音量条
        noVolume: false,
        // 不显示进度条
        noProcess: false,
        // 只能播放一个
        onlyOnePlaying: false,
        // 不要快进按钮
        noSpeed: false
      }
    };
  },
  mounted() {
    // console.log(this.musicsrc);
  },
  methods: {
    startPlayOrPause() {
      return this.audio.playing ? this.pausePlay() : this.startPlay();
    },
    // 开始播放
    startPlay() {
      this.$refs.audio.play();
    },
    // 暂停
    pausePlay() {
      this.$refs.audio.pause();
    },
    onError() {
      this.audio.waiting = true;
    },
    onPause() {
      this.audio.playing = false;
    },
    onPlay(res) {
      console.log(res);
      this.audio.playing = true;
      this.audio.loading = false;
    }
    // pauseAudio() {
    //   const x = document.getElementById("myAudio");
    //   if (x.paused) {
    //     x.play();
    //   } else {
    //     x.pause();
    //   }
    // }
  }
};
</script>

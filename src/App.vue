<template>
  <div id="app">
    <header>
      <h1>我的音乐</h1>
    </header>
    <main>
      <!-- 正在播放 -->
      <section class="player">
        <!-- 当前播放的音乐展示 -->
        <h2 class="song-title">
          {{ current.title }}-
          <span>{{ current.artist }}</span>
        </h2>
        <!-- 添加播放控件 -->
        <div class="controls">
          <button class="prev" @click="prev">上一首</button>
          <!-- 如果音乐正在播放 不需要播放键 -->
          <button class="play" v-if="!isPlaying" @click="play(current)">
            播放
          </button>
          <button class="pause" v-else @click="pause">暂停</button>
          <button class="next" @click="next">下一首</button>
        </div>
      </section>
      <!-- 播放列表 -->
      <section class="playlist">
        <h3 class="song-title">播放列表</h3>
        <button
          v-for="song in songs"
          :key="song.src"
          @click="play(song)"
          :class="song.src == current.src ? 'song playing' : 'song'"
        >
          {{ song.title }} - {{ song.artist }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      //当前播放的歌曲
      current: {},
      index: 0,
      // 是否正在播放音乐
      isPlaying: false,
      //歌曲列表 数组表示
      songs: [
        {
          //歌名
          title: "This year",
          // 歌手名
          artist: "10cm",
          src: require("./assets/10cm-this year.mp3"),
        },
        {
          //歌名
          title: "Love is gone",
          // 歌手名
          artist: "Angesi",
          src: require("./assets/Angesi-Love is Gone .mp3"),
        },
      ],
      // audio：HTML5新标签
      player: new Audio(),
    };
  },
  methods: {
    play(song) {
      // 有音乐可播放
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
        // 播放
        this.player.play();
        this.isPlaying = true;
      }

      // 如果一首音乐播放完毕，自动跳转到下一首
      this.player.addEventListener(
        "ended",
        function () {
          this.index++;
          if (this.index > this.songs.length - 1) {
            this.index = 0;
          }
          this.current = this.songs[this.index];
          this.play(this.current);
        }.bind(this)
      );
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    next() {
      this.index++;
      //不能超过
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev() {
      this.index--;
      //不能超过
      if (this.index < 0) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
  },
  created() {
    // console.log(this.playe);
    // 将当前歌曲列表中的第一首歌赋值给当前播放的歌曲
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
    //组件一旦挂载 立即播放音乐
    // this.player.play()
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
}
header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  color: #fff;
  background-color: #212121;
}

main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}

.song-title {
  color: #53565a;
  font-size: 32px;
  /* 字体粗细 */
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}

.song-title span {
  font-weight: 400;
  font-style: italic;
}

.controls {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 30px 15px;
}

button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}

button:hover {
  opacity: 0.8;
}

.play, .pause{
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  background-color: #cc2e5d;
  color: #fff;
  border-radius: 8px;
}

.next,
.prev {
  font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 15px;
  background-color: #ff5858;
  color: #fff;
  border-radius: 8px;
}

.playlist{
  padding: 0px 30px;
}

.playlist h3{
  color: #212121;
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 30px;
  text-align: center;
}

.playlist .song{
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}

.playlist .song:hover{
  color: #ff5858;
}
.playlist .song.playing{
  color: #FFF;
  background-image: linear-gradient(to right,#CC2E5D,#ff5858);
}
</style>

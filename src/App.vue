<template>
  <div id="app">
    <header>
      <div class="container-fluid">
        <svg class="bi bi-music-note-beamed" width="50px" height="50px" viewBox="0 0 16 16" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
        <path d="M6 13c0 1.105-1.12 2-2.5 2S1 14.105 1 13c0-1.104 1.12-2 2.5-2s2.5.896 2.5 2zm9-2c0 1.105-1.12 2-2.5 2s-2.5-.895-2.5-2 1.12-2 2.5-2 2.5.895 2.5 2z"/>
        <path fill-rule="evenodd" d="M14 11V2h1v9h-1zM6 3v10H5V3h1z"/>
        <path d="M5 2.905a1 1 0 0 1 .9-.995l8-.8a1 1 0 0 1 1.1.995V3L5 4V2.905z"/>
      </svg>
      <h1>Max Music</h1>
      </div>
    </header>
    <div class="border__header"></div>
    <section class="player">
      <div class="container">
        <h2 class="song-title">{{ current.title }} - <span>{{ current.artist }}</span></h2>
        <div class="control">
          <button class="prev btn btn-outline-info btn-sm" @click="prev">Prev</button>
          <button class="play btn btn-danger" v-if="!isPlaying" @click="play">Play</button>
          <button class="pause btn btn-success" v-else @click="pause">Pause</button>
          <button class="next btn btn-outline-info btn-sm" @click="next">Next</button>
        </div>
      </div>
    </section>
    <div class="border__header"></div>
    <section class="playlist">
      <div class="container">
        <h3>The PlayList</h3>
      </div>
      <button v-for="song in songs" :key="song.src" @click="play(song)" 
      :class="(song.src == current.src) ? 'song playing' : 'song'">
        {{ song.title }} - {{ song.artist }}
      </button>
    </section>
  </div>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
        title: 'Buttons',
        artist: 'The Pussycat Dolls',
        src: require('./assets/The-Pussycat-Dolls_Buttons-ft-Snoop-Dogg.mp3')
        },
        {
        title: 'Sexy Back',
        artist: 'Justin Timberlake',
        src: require('./assets/Justin Timberlake - Sexy Back_(muzfan.net).mp3')
        },
        {
        title: 'Stronger',
        artist: 'Kanye West',
        src: require('./assets/Kanye West - Stronger.mp3')
        },
        {
        title: 'I Touch Myself',
        artist: 'Scala & Kolacny Brothers',
        src: require('./assets/Scala & Kolacny Brothers - I Touch Myself (QST сериал Sex Education )_(x-Minusovka.ru).mp3')
        }
      ],
      player: new Audio()
    }
  }, 
  methods: {
    play (song) {
      if (typeof song.src != 'undefined') {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener('ended', function() {
        this.index++;
        if (this.index > this.songs.length - 1) {
          this.index = 0;
        }

        this.current = this.songs[this.index]
        this.play(this.current)
      }.bind(this))
      this.isPlaying = true;
    },
    pause () {
      this.player.pause()
      this.isPlaying = false
    },
    next () {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }

      this.current = this.songs[this.index]
      this.play(this.current)
    },
    prev () {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }

      this.current = this.songs[this.index]
      this.play(this.current)
    }
  },
  created () {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
    // this.player.play();

  }
}

</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: sans-serif;
  background-color: #13152a !important;
  color: #44bbff !important;
}
header div {
  display: flex !important;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
  background-color: #13152a;
  color: #fff;
}

.border__header {
  width: 100%;
  height: 3px;
  background-image: linear-gradient(90deg,#8226b7,#00a2b5);
}

.player {
  padding: 30px 0;
  background: #1b213b;
}

.song-title {
  text-align: center;
  width: 100%;
  margin-bottom: 30px;
}

.song-title span {
  opacity: 0.6;
}

.control {
  display: flex;
  justify-content: center;
  align-items: center;
}

.play, .pause {
  margin: 0 5px;
  width: 70px;
  min-width: 70px;
  outline: 0;
}

.playlist {
  margin-top: 30px;
}

.playlist h3 {
  color: #fff;
  text-align: center;
  margin-bottom: 30px;
}

.song {
  display: block;
  width: 100%;
  border: none;
  color: #f3f4fb;
  background: #838fc0;
  padding: 10px 0;
  outline: none;
  box-shadow: none;
}

.song:focus {
  border: none;
  box-shadow: none;
  outline: none;
}

.song.playing {
  background-image: linear-gradient(90deg,#8226b7,#00a2b5);
  color: #fff;
}


</style>

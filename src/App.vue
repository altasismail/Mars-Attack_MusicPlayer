<template>
  <div id="app">
    <header>
      <h1>
        Mars-Attack Musics
      </h1>

    </header>
    <main>
      <section class="player">
        <h2 class="song-title">{{current.title}} - <span>{{current.artist}}</span>
        </h2>
        <!-- /*{{songs[0].src}}*/ to see the source working compile -->
        <div class="controls">
          <button class="prev" @click="prev">Prev</button>
          <button class="play" v-if="!isPlaying" @click="play">Play</button>
          <button class="pause" v-else @click="pause">Pause</button>
          <button class="next" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>The Play List</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">{{song.title}} - {{song.artist}}</button>


      </section>
    </main>

  </div>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      current: {
        // title: "SONG TITLE" no longer needed because of the changing titles of the music
      },
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: "Bir Derdim Var",
          artist: "Mor ve Ötesi",
          src: require("./assets/Mor ve Ötesi - Bir Derdim Var-128.mp3")

        },
        {
          title: "Cambaz",
          artist: "Mor ve Ötesi",
          src: require("./assets/Mor ve Ötesi - Cambaz-128.mp3")
        },
        {
          title: "Gemiler",
          artist: "Teoman",
          src: require("./assets/Teoman - Gemiler-128.mp3")
        },
        {
          title: "Gönülçelen",
          artist: "Teoman",
          src: require("./assets/Teoman - Gönülçelen -128.mp3")
        }
      ],
      player: new Audio()
    }
  },
  methods: {
    play (song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener('ended', function () {
        this.index++;
        if  (this.index > this.songs.length - 1){
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);

      }.bind(this));
      this.isPlaying = true;
    },
    pause () {
      this.player.pause();
      this.isPlaying = false;
    },
    next () {
      this.index++;
      if  (this.index > this.songs.length - 1){
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev () {
      this.index--;
      if  (this.index < 0 ){
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created () {
    this.current = this.songs[this.index];
    this.player.src = this.current.src; 
    // this.player.play(); this code will play the current music.
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
  font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;

}
header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 16px;
  background-color: rgb(245, 112, 59);
  color: #ffffff;
}
main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;

}

.song-title {
  color:  rgb(211, 98, 5);
  font-size: 32px;
  font-weight: 700;
  text-transform: capitalize;
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
  padding: 32px 16px;
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

.play, .pause {
  font-size: 22px;
  font-weight: 700;
  padding: 12px 32px;
  margin: 0px 1px;
  border-radius: 8px;
  color: #ffffff;
  background-color: rgb(211, 98, 5);

}
.next, .prev {
  font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 1px;
  border-radius: 6px;
  color: #ffffff;
  background-color: rgb(74, 121, 128);
}
.playlist {
  padding: 0px 30px;

}
.playlist h3 {
  color: rgb(46, 83, 68);
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 30px;
  text-align: center;
 
}
.playlist .song {
  display: block;
  width: 100%;
  padding: 14px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;

}

.playlist .song:hover {
  color: rgb(74, 121, 128);
}
.playlist .song.playing {
  color: #ffffff;
  background-image: linear-gradient(to right, rgb(74, 121, 128),  rgb(19, 78, 87));
}

</style>

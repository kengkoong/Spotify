<template>
  <div id="app">
    <header>
      <h1>K-MUSIC</h1>
    </header>
    <main>
      <section class="player">
        <h3 class="song-title">
          {{current.title}} - <span>{{current.artist}}</span>
        </h3>
        <div class="control">
          <button class="prev" @click="prev"><ion-icon name="play-back-outline"></ion-icon></button>
          <button class="play" v-if="!isPlaying" @click="play"><ion-icon  class="play-icon" name="play-circle-outline"></ion-icon></button>
          <button class="pause" v-else @click="pause"><ion-icon class="play-icon" name="pause-circle-outline"></ion-icon></button>
          <button class="next" @click="next"><ion-icon name="play-forward-outline"></ion-icon></button>
        </div>
        
      </section>

      <section class="playlist">
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
          {{ song.title }} - {{ song.artist }}
        </button>
      </section>
    </main>
    
  </div>
</template>

<script>
export default {
  name: 'App',
  data(){
    return {
      current:{},
      index:0,
      isPlaying:false,
      songs:[
        {
          title:'ลงใจ',
          artist:'BOWKYLION',
          src:require('./assets/music1.mp3')
        },
        {
          title:'ดึกแล้วอย่าพึ่งกลับ',
          artist:'Jaonay',
          src:require('./assets/music2.mp3')
        }
      ],
      player:new Audio()
    }
  },
  methods:{
    play (song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener('ended', function () {
        this.index++;
        if (this.index > this.songs.length - 1) {
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
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev () {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    }
  
  },
  created(){
    this.current=this.songs[this.index];
    this.player.src=this.current.src;
    //this.player.play();
  }
}
</script>

<style>
*{
  margin:0;
  padding:0;
  box-sizing: border-box;
}
body{
  background-color:#000;
}
header{
  display: flex;
  justify-content: center;
  align-items: center;
  background-color:darkred;
  color:#fff;
  padding:15px;
}
main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
  
}
.song-title {
  color: #fff;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}
.song-title span {
  font-weight: 400;
  font-style: italic;
}
.control {
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
.play, .pause {
  
  padding-left:10px;
  padding-right:10px;
  padding-top:5px;
  padding-bottom:5px;
  margin: 0px 15px;
  border-radius: 8px;
  color: #CC2E5D;
  border:2px solid #CC2E5D;
  
}
.next, .prev {
  font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color: #FF5858;  
  border:2px solid #FF5858;
}
.playlist {
  padding: 0px 30px;
}
.playlist h3 {
  color: #212121;
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 30px;
  text-align: center;
}
.playlist .song {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
  color:#fff;
  
}
.playlist .song:hover {
  color: #FF5858;
}
.playlist .song.playing {
  color: #FFF;
  background-image: linear-gradient(to right, #CC2E5D, #FF5858);
}
.play-icon{
  font-size:2em;  
}
</style>

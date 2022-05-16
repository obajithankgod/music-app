<template>
 <div class="app">
   <header>
     <h1>Music App</h1>
   </header>
   <main>
     <section class="player">
        <h1 class="song-title">{{ current.artist }} -- <span>{{ current.title }}</span></h1>
       <div class="controls">
         <button class="prev" @click="prev">prev</button>
         <button class="play" v-if="!isPlaying" @click="play">play</button>
         <button class="pause" v-else @click="pause">pause</button>
         <button class="next" @click="next">next</button>
       </div>
     </section>
     <section class="playlist">
       <h3>My Playlist</h3>
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
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title:'Godly',
          artist: 'Omah_lay',
          src: require('./assets/Omah_Lay_-_Godly.mp3')
        },
        {
          title: 'Hello_Brother',
          artist: 'Omah_lay',
          src: require('./assets/Omah_Lay_-_Hello_Brother(128k).mp3')
        },
        {
          title: 'Bad-influence',
          artist: 'Omah_lay',
          src: require('./assets/Omah-Lay-Bad-Influence-(JustNaija.com).mp3')
        }
        
      ],
      player: new Audio()
    }
  },

  methods: {
    play (song) {
      if (typeof song.src !="undefined") {
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

      }.bind(this))
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
      if (this.index < 0)
      this.index = this.songs.length - 1;

      this.current = this.songs[this.index];
      this.play(this.current)

    }

  },
  created () {
        this.current = this.songs[this.index];
        this.player.src = this.current.src;
    }
  
}
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
body {
  font-family: sans-serif;
  background: linear-gradient(35deg, #ccffff, #ffcccc);
}
 header {
   display: flex;
   justify-content: center;
   align-content: center;
   padding: 15px;
   background-color: #212121;
   color: #fff ;
    
 }
 main {
   width: 100%;
   max-width: 768px;
   margin: 0 auto;
   padding: 25px;
 }
 .song-title {
   color: #9591b1;
   font-size: 20px;
   font-weight: 700;
   text-transform: uppercase;
   text-align: center;
 }
 .song-title span {
   font-weight: 400;
   font-style: italic;
   font-size: 20px;
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
button:hover{
  opacity: 0.8;
}
.play, .pause {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: #fff;
  background-color: #cc2e5d;
 box-shadow: 1px 1px 10px lightblue;
}
.next, .prev {
  font-size: 15px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color: #fff;
  background-color: #011b2c;
  box-shadow: 1px 1px  5px lightblue;

}
.playlist {
  padding: 0px 15px;
}
.playlist h3 {
  color: #fff;
  font-size: 28px;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  font-weight: 400;
  margin-bottom: 30px;
 
}
.playlist .song {
  display: block;
  width: 100%;
  padding: 10px;
  font-size: 20px;
  font-weight: 700;
  color: #14e7dd;
  cursor: pointer;
}

.playlist .song.playing {
  color: #fff;
  background-image: linear-gradient(to right, #cc2e5d, #ff5858);
}
.playlist .song:hover {
  color: #ff5858;
}
</style>

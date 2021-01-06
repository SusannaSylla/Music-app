<template>
  <div id="app">
    <header>
      <a href="#" id="logo_firstname" class="logo_name"><span style="font-size: 48px; font-weight: 500;">S</span>USANNA</a>
      <a href="#" id="logo_lastname" class="logo_name"><span style="font-size: 48px; font-weight: 500;">S</span>AADI</a>
      <div class="logo_stripes" id="upper_stripe"></div>
      <div class="logo_stripes" id="lower_stripe"></div>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">{{ current.title }} - <span>{{ current.artist }}</span></h2>
        <div class="controls">
          <button class="prev" @click="prev">Prev</button>
          <button class="play" v-if="!isPlaying" @click="play">Play</button>
          <button class="pause" v-else @click="pause">Pause</button>
          <button class="next" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>The playlist</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src)
  ? 'song playing' : 'song'">
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
          title: 'Bring me back to life',
          artist: 'InfiNoise',
          src: require('./assets/InfiNoise - Bring me back to life (Feat. DNAKM) [NCS Release].mp3')
        },
        {
          title: 'On & On',
          artist: 'Cartoon',
          src: require('./assets/Cartoon - On & On (feat. Daniel Levi) [NCS Release].mp3')
        }
      ],
      player: new Audio()
    }
  },
  methods: {
    play(song) {
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


    pause() {
      this.player.pause();
      this.isPlaying = false;
    },

    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },

    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    }

  },

  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  }

}
</script>

<style>

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  background-image: linear-gradient(#062041 0%, #020B17 100%);
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
}

body {
  font-family: sans-serif;


}

header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 6em;
  color: white;
  font-size: 1.5em;

}

main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;

}

.song-title {
  color: white;
  font-size: 3em;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}

.song-title span {
  font-weight: 400;
  font-size: 0.8em;
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

.play, .pause {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: #FFF;
  background-color: #CC2E5D;
}

.next, .prev {
  font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color: #FFF;
  background-color: #CC2E5D;
}

.playlist {
  padding: 0px 30px;
}

.playlist h3 {
  color: #FFF;
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
  color: #495562;
}

.playlist .song:hover {
  color: #FF5858
}

.playlist .song.playing {
  color: #FFF;
  background-image: linear-gradient(to right, #CC2E5D, #FF5858)
}

.logo_name {
  color: white;
  font-family: 'Montserrat', sans-serif;
  font-weight: 400;
  font-size: 36px;
  cursor: pointer;
  text-decoration: none;
}

#logo_firstname {
  position: absolute;
  left: 5%;
  top: 7%;
}

#logo_lastname {
  position: absolute;
  top: 13%;
  left: 11.5%;
}

#upper_stripe {
  position: absolute;
  width: 15%;
  height: 7px;
  left: 7.5%;
  top: 19.5%;
  background-color: #B53D4B;
}

#lower_stripe {
  position: absolute;
  width: 15%;
  height: 7px;
  left: 2%;
  top: 21.5%;
  background-color: #B53D4B;
}

@media only screen and (max-width: 900px) {
  #logo_firstname {
    left: 6%;
    top: 5%;
  }

  #logo_lastname {
    top: 9%;
    left: 24%;
  }

  #upper_stripe {
    width: 30%;
    left: 15%;
    top: 14%;
  }

  #lower_stripe {
    width: 30%;
    left: 4%;
    top: 15.5%;
  }
}

@media only screen and (max-width: 700px) {
  header {
    padding: 4em;
  }

  #upper_stripe {
    width: 40%;
    left: 15%;
    top: 17%;
    height: 5px;
  }

  #lower_stripe {
    width: 36%;
    left: 4%;
    top: 18.5%;
    height: 5px;
  }

  .logo_name {
    font-size: 30px;
    cursor: pointer;
    text-decoration: none;
  }

  #logo_firstname {
    position: absolute;
    left: 5%;
    top: 3.5%;
  }

  #logo_lastname {
    position: absolute;
    top: 9.5%;
    left: 25%;
  }
}
@media only screen and (min-width: 900px) {
  html {
    height: 100vh;
  }
}
</style>

<template>
  <div class="container">
    <header>
      <img src="./assets/howlinglogo.png" alt="">
    </header>
    <main>
      <PlayerSongs/>
    </main>
  </div>
</template>

<script>
import PlayerSongs from "./components/PlayerSongs.vue"

export default {
  name: 'App',
  components: {
    PlayerSongs
  },
  /*data() {
    return {
      songs: [
        {
          title: "Crow Song",
          artist: "Lisa",
          src: require('./assets/Crow-Song.mp3')
        },
        {
          title: "Alchemy",
          artist: "Lisa",
          src: require('./assets/Alchemy.mp3')
        }
      ]
    }
  }*/
}
</script>

<style lang="scss">
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body{
  font-family: sans-serif;
}

.container {
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  margin-top: 25px;

  header {
    width: 140%;
    padding: 25px;
    img {
      width: 100%;
    }
  }
  main {
    width: 100%;
    margin: 0 auto;
    padding: 25px;
  }
}

/*@media (max-width: 1900px) {
      .container {
      
        header {
          img{
            width: 100%;
          }
        }
      }
    }*/

@media (max-width: 800px) {
      .container {
        display: flex;
        flex-direction: column;

        header {
          width: 100%;
        }
      }
    }

</style>

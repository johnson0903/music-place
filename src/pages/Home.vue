<template>
  <div class="home">
    <div class="list">
      <h1>播放清單</h1>
      <ul>
        <li v-for="(album, index) in albums" :key="album.cover">
          <img
            class="album-cover"
            @click="selectAlbum(index)"
            :src="album.cover"
            alt="album.cover"
          />
        </li>
      </ul>
    </div>
    <div class="player-container">
      <Player :album="selectedAlbum" />
    </div>
  </div>
</template>

<script>
import Player from '../components/Player';
export default {
  name: 'Home',
  components: {
    Player,
  },
  props: {
    msg: String,
  },
  data() {
    return {
      selectedAlbum: null,
      albums: [
        {
          cover: require('../assets/黑色台北(Urban Noir)/cover.jpg'),
          tracks: [
            {
              name: '01 - 墜落',
              artist: '賀爾蒙少年',
              src: require('../assets/黑色台北(Urban Noir)/01 - 墜落.mp3'),
              url: 'https://www.youtube.com/watch?v=z3wAjJXbYzA',
              favorited: false,
            },
            {
              name: '02 - 中華商場1971 feat.修齊',
              artist: '賀爾蒙少年',
              src: require('../assets/黑色台北(Urban Noir)/02 - 中華商場1971 feat.修齊.mp3'),
              url: 'https://www.youtube.com/watch?v=z3wAjJXbYzA',
              favorited: false,
            },
          ],
        },
        {
          cover: require('../assets/Lady Gaga - Chromatica/cover.jpg'),
          tracks: [
            {
              name: '01 - Chromatica I',
              artist: 'Lady Gaga',
              src: require('../assets/Lady Gaga - Chromatica/01 - Chromatica I.flac'),
              url: 'https://www.youtube.com/watch?v=z3wAjJXbYzA',
              favorited: false,
            },
            {
              name: '02 - Alice',
              artist: 'Lady Gaga',
              src: require('../assets/Lady Gaga - Chromatica/02 - Alice.flac'),
              url: 'https://www.youtube.com/watch?v=z3wAjJXbYzA',
              favorited: false,
            },
          ],
        },
      ],
    };
  },
  created() {
    this.selectedAlbum = this.albums[0];
  },
  methods: {
    selectAlbum(index) {
      this.selectedAlbum = this.albums[index];
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.home {
  display: flex;
}
.list {
  width: 20%;
  display: flex;
  flex-direction: column;
  align-items: center;
  ul {
    list-style: none;
    li {
      margin-bottom: 15px;
      :hover {
        cursor: pointer;
      }
    }
  }
}
.player-container {
  display: flex;
  justify-content: center;
  width: 80%;
  height: 100vh;
  background-color: #f7f2dd;
}
.album-cover {
  width: 150px;
  box-shadow: 0px 8px 20px 0px gray;
}
</style>

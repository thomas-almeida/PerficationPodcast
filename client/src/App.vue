<script>

export default {
  data() {
    return {
      inputValue: '',
      searchData: null,
      episodeArt: '/default-art.png',
      episodeTitle: 'Selecione um Episódio',
      episodeAudio: ''
    }
  },
  methods: {

    // funções do componente

    podFetch() {

      fetch(`https://spotify23.p.rapidapi.com/search/?q=${this.inputValue}&type=episodes&limit=100`, {
        headers: {
          'X-RapidAPI-Key': '0a8025daa0msh1827d8b09fb2a34p1d1a3bjsn017c5eee4962',
          'X-RapidAPI-Host': 'spotify23.p.rapidapi.com'
        }
      })
        .then(response => response.json())
        .then(data => {
          console.log(data)
          this.searchData = data
        })

    },

    cowsay(event) {

      const newEpisodeTitle = event.target.parentNode.childNodes[1].childNodes[0].innerText
      const episodeID = event.target.parentNode.childNodes[1].childNodes[1].innerText.split(":")
      this.episodeArt = event.target.src
      this.episodeTitle = newEpisodeTitle

      // Episode ID
      console.log(episodeID[2])

      fetch(`https://spotify23.p.rapidapi.com/episode_sound/?id=${episodeID[2]}`, {
        headers: {
          'X-RapidAPI-Key': '0a8025daa0msh1827d8b09fb2a34p1d1a3bjsn017c5eee4962',
          'X-RapidAPI-Host': 'spotify23.p.rapidapi.com'
        }
      })
        .then(response => response.json())
        .then(data => {
          console.log(data)
          this.episodeAudio = data.url[1]
        })

    }

  }
}

</script>

<template>
  <nav>
    <section>
      <input type="text" placeholder="Pesquisar" v-model="inputValue" @keyup.enter="podFetch">
      <ion-icon id="searchIcon" name="search-outline"></ion-icon>
    </section>
  </nav>
  <main>
    <div class="content" v-if="searchData">
      <ul>
        <li v-for="episode in searchData.episodes.items" :key="episode.data">
          <img :src="episode.data.coverArt.sources[1].url" :alt="episode.data.name" @click="cowsay($event)"
            draggable="false">
          <section>
            <h3>{{ episode.data.name }}</h3>
            <p>{{ episode.data.uri }}</p>
          </section>
        </li>
      </ul>
    </div>
    <div class="player">
      <img :src="episodeArt" alt="Arte do Episódio" draggable="false">
      <section class="ep-info">
        <h3>{{ episodeTitle }}</h3>
      </section>
      <br>
      <section class="audio-player">
        <section>
          <audio :src="episodeAudio" preload="auto" controls></audio>
        </section>
      </section>

    </div>
  </main>
</template>

<style scoped></style>

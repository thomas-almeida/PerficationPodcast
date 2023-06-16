<script>

export default {
  data() {
    return {
      inputValue: '',
      searchData: null,
      episodeArt: '/album-model.jpg',
      episodeTitle: 'Selecione um Episódio',
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

    cowsay(event){
      this.episodeArt = event.target.src
      let newEpisodeTitle = event.target.parentNode.childNodes[1].childNodes[0].innerText
      this.episodeTitle = newEpisodeTitle

    }

  }
}

</script>

<template>
  <nav>
    <section>
      <input type="text" placeholder="..." v-model="inputValue" @keyup.enter="podFetch">
      <ion-icon id="searchIcon" name="search-outline"></ion-icon>
    </section>
  </nav>
  <main>
    <div class="content" v-if="searchData">
      <ul>
        <li v-for="episode in searchData.episodes.items" :key="episode.data">
          <img :src="episode.data.coverArt.sources[1].url" :alt="episode.data.name" @click="cowsay($event)" draggable="false">
          <section>
            <h3>{{ episode.data.name }}</h3>
          </section>
        </li>
      </ul>
    </div>
    <div class="player">
      <img :src="episodeArt" alt="Arte do Episódio" draggable="false">
      <section class="ep-info">
        <h3>{{episodeTitle}}</h3>
      </section>
      <br>
      <section class="audio-player">
        <section class="audio-buttons">
          <ion-icon name="play-skip-back"></ion-icon>
          <ion-icon name="play"></ion-icon>
          <ion-icon name="play-skip-forward"></ion-icon>
        </section>
        <section>
          <progress value="100"></progress>
        </section>
      </section>

    </div>
  </main>
</template>

<style scoped></style>

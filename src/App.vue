<template>
    <div id="app">
        <div class="search-container row justify-content-center mt-5">
            <h1>buscaLETRAS</h1>
        </div>
        <div class="search-container row justify-content-center">
            <form id="lyrics_form">
                <div class="form-row">
                    <div class="form_group col-md-6">
                        <label for="artist">Artista</label>
                        <input 
                          type="text" 
                          class="form-control" 
                          v-model.trim="artista"
                          placeholder="Ex: Djavan"
                        /></div>

                    <div class="form_group col-md-6">
                        <label for="song">Música</label>
                        <input 
                          type="text" 
                          class="form-control" 
                          id="song" 
                          v-model.trim="musica" 
                          placeholder="Ex: Oceano"
                        /></div>
                </div>
                <button type="submit" @click.prevent="buscaLetra" class="btn btn-primary mt-2">busca LETRA</button>
            </form>
        </div>
        <div class="lyrics-container row justify-content-center">
          <div class="spinner-grow text-primary mt-5" role="status" v-if="esperando"></div>
          <pre class="mt-4" id="lyrics" v-else>{{lyrics}}</pre>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      artista:'',
      musica: '',
      lyrics:'',
      esperando: false
    }
  },
  methods: {
    async buscaLetra() {
      this.esperando = true
      if (this.artista == '' || this.musica == '') {
        this.esperando = false
        this.lyrics = 'Preencha todos os campos!'
      } else {
        await axios.get(`https://api.lyrics.ovh/v1/${this.artista}/${this.musica}`, {
          timeout: 31000
        })
        .then(res => {
          if (res.data.lyrics) {
            this.lyrics = res.data.lyrics
            this.esperando = false
            this.artista = ''
            this. musica = ''
          }
        })
        .catch(err => {
          this.esperando = false
          this.artista = ''
          this. musica = ''
          this.lyrics = `Oooops!!! Não foi possível encontrar ${this.artista} ou a música ${this.musica}!`
        })
      }
    }
  }
};
</script>


<style lang="scss">
#app {
  margin: auto;
  width: 88%;

  h1 {
    font-family: 'Pattaya', sans-serif;
  }
  .btn {
    font-family: 'Pattaya', sans-serif;
  }

  ::-webkit-input-placeholder {
   color: rgba(128, 128, 128, 0.349);
  }

  .search-container {
    background-color: rgba(211, 211, 211, 0.431);
    padding: 10px;
  }

  .lyrics-container {
    background-color: #fff;
    display: flex;
    flex-wrap: nowrap;
  }
}
</style>
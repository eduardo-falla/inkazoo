<template>
  <!-- Loading -->
  <Loading v-if="$fetchState.pending" />

  <!-- Movie Info -->
  <div v-else class="single-movie container">
    <NuxtLink class="button" :to="{ name: 'index' }"> Back </NuxtLink>
    <div class="movie-info">
      <div class="movie-img">
         <video ref="Playerjs" class="video-js" height="360" width="720"></video>
      </div>
      <div class="movie-content">
        <h1>Especie: {{ movie.especie }}</h1>
        <p class="movie-fact tagline">
          <span>Descripción:</span>  {{ movie.descripcion }}
        </p> 
        <p class="movie-fact">
          <span>Variedad:</span>   {{ movie.variedad }}
        </p>
        <p class="movie-fact">
          <span>Origen:</span>   {{ movie.origen }} 
        </p>
        <p class="movie-fact">
          <span>Precio:</span>   {{ movie.precio }}
 
        </p>
        <p class="movie-fact"><span>Nivel de cuidado:   </span> {{ movie.nivel_cuidado }}</p>
        <p class="movie-fact"><span>Más información:   </span>  <a :href="movie.masinfo">Clic aquí</a></p>
        <Watsap :cosa="movie.especie"/>
      </div>
      
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import videojs from 'video.js';
import 'video.js/dist/video-js.css';
import Watsap from '/components/Watsap.vue';




export default {
  components: { Watsap },
  name: 'singleMovie',


  async fetch() {
    await this.getSingleMovie()
  },

  // delay for fetching
  fetchDelay: 1000,


mounted() {
  const interval = setInterval(() => {
      if (this.$refs.Playerjs) {

this.player = videojs(this.$refs.Playerjs, {
      autoplay: true,
      controls: true,
      techOrder: ['html5'],
      sourceOrder: true,
      html5: {
        hls: {
          withCredentials: false ,
        },
      },
     sources: [
        {
          type: 'application/x-mpegURL',
          src: 'http://0.0.0.0:8080/hls/test1.m3u8',
          
        },
      ],
    })

        clearInterval(interval)
      }
    }, 50)

console.log("route_params:", this.$route.params.movieid);



},

  beforeDestroy() {
    if (this.player) {
      this.player.dispose();
    }
    this.player = null;
  },






  head() {
    return {
      title: this.movie.especie,
    }
  },

  data() {
    return {
      movie: '',
      player: null,
      tagvideo: null,
    }
  },
  methods: {
    async getSingleMovie() {
      const data = axios.get(
        `http://localhost:3001/todos?id=${this.$route.params.movieid}`
      )
      const result = await data
      this.movie= result.data[0]
      console.log("route_params:", this.$route.params.movieid);
      console.log("movie loading:", this.movie);
    },
  },
}
</script>



<style lang="scss">
.single-movie {
  color: #fff;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 32px 16px;

  .button {
    align-self: flex-start;
    margin-bottom: 32px;
  }

  .movie-info {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 32px;
    color: #fff;
    @media (min-width: 800px) {
      flex-direction: row;
      align-items: flex-start;
    }
    .movie-img {
      img {
        max-height: 500px;
        width: 100%;

        @media (min-width: 800px) {
          max-height: 700px;
          width: initial;
        }
      }
    }



    .movie-content {
      h1 {
        font-size: 32px;
        font-weight: 400;
      }

      .movie-fact {
        margin-top: 12px;
        font-size: 16px;
        line-height: 1.5;

        span {
          font-weight: 600;
          /* text-decoration: underline; */
        }

        a {
          font-weight: 600;
          text-decoration: none;
          color: #fff;
        }
      }

      .tagline {
        font-style: italic;
        span {
          font-style: normal;
        }
      }
    }
  }
}
</style>

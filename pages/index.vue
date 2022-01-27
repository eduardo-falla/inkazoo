<template>
  <div class="home">
    <!-- Hero -->
    <Hero />

    <!-- Search -->
    <div class="container search">
      <input
        type="text"
        placeholder="Search"
        @keyup.enter="$fetch"
        v-model.lazy="searchInput"
      />
      <button v-show="searchInput !== ''" @click="clearSearch" class="button">
        Clear Search
      </button>
    </div>

    <!-- Loading Animation -->
    <Loading v-if="$fetchState.pending" />

    <!-- Movies -->
    <div v-else class="container movies">
      <!-- Search Results -->
      <div id="movie-grid" v-if="searchInput !== ''" class="movies-grid">
        <div
          class="movie"
          v-for="(movie, index) in searchedMovies"
          :key="index"
        >
          <div class="movie-video">
          <VideoPlayer :fuente="movie.url" :key="movie.url"/>
<!--             <p class="review">Stock:{{ movie.stock }}</p> -->
            <p class="overview">{{movie.precio}}</p>
            
          </div>
          <div class="info">
           
            <p class="title">
              {{ movie.especie.slice(0, 25)
              }}<span v-if="movie.id.length > 25">...</span>
            </p>
            <p class="release">
              stock:
            {{ movie.stock}}
            </p>
            <NuxtLink
              class="button button-light"
              :to="{ name: 'movies-movieid', params: { movieid: movie.id } }"
            >
              En vivo
            </NuxtLink>
          </div>
        </div>
      </div>

      <!-- Now Streaming  -->
      <div id="movie-grid" v-else class="movies-grid">
        <div class="movie" v-for="(movie, index) in movies" :key="index">
          <div class="movie-video">
          <VideoPlayer :fuente="movie.url" :key="movie.url"/>
           <!--  <p class="review">Stock: {{ movie.stock }}</p> -->
            <p class="overview">{{movie.precio}}</p>
            
          </div>
          <div class="info">
            
            <p class="title">
              {{ movie.especie.slice(0, 25)
              }}<span v-if="movie.especie.length > 25">...</span>
            </p>
            <p class="release">
              Stock:
            {{ movie.stock}}
            </p>
            <NuxtLink
              class="button button-light"
              :to="{ name: 'movies-movieid', params: { movieid: movie.id } }"
            >
              En vivo
            </NuxtLink>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import videojs from 'video.js';
import axios from 'axios'
import VideoPlayer from "/components/VideoPlayer.vue";

export default {
  name: 'home',
  components: {
		VideoPlayer
	},
  head() {
    return {
      title: 'Movie App - Latest Streaming Movie Info',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'Get all the latest streaming movies in theaters & online',
        },
        {
          hid: 'keywords',
          name: 'keywords',
          content: 'movies, stream, stremaing',
        },
      ],
    }
  },

mounted(){

},


  data() {
    return {
      movies: [],
      searchedMovies: [],
      searchInput: '',

    }
  },

  async fetch() {

    if (this.searchInput === '') {
      this.movies= []
      await this.getMovies()
      return
    }

    if (this.searchInput !== '') {
      
      await this.searchMovies()
    }
  },
  fetchDelay: 1000,

  methods: {
    async getMovies() {
      const data = axios.get(
        `http://localhost:3001/todos`
      )
      const result = await data
      console.log("resultado:", result.data);
      result.data.forEach((movie) => {
        this.movies.push(movie);
})
console.log("movies:",this.movies);
    },

    async searchMovies() {
      const data = axios.get(
        `http://localhost:3001/todos?q=${this.searchInput}`
      )
      const result = await data
      console.log(result);
      result.data.forEach((movie) => {
      this.searchedMovies.push(movie)
      console.log(this.searchedMovies)
      })
    },

    clearSearch() {
      this.searchInput = ''
      this.searchedMovies = []
      console.log(this.movies)
      fetch()
    },
  },





  watch: {
    searchInput() {
      console.log(this.searchInput)
    },
  },
}
</script>




<style lang="scss">
.home {
  .loading {
    padding-top: 120px;
    align-items: flex-start;
  }

  .search {
    display: flex;
    padding: 32px 16px;

    input {
      max-width: 350px;
      width: 100%;
      padding: 12px 6px;
      font-size: 14px;
      border: none;

      &:focus {
        outline: none;
      }
    }

    .button {
      border-top-left-radius: 0;
      border-bottom-left-radius: 0;
    }
  }

  .movies {
    padding: 32px 16px;
    .movies-grid {
      display: grid;
      column-gap: 32px;
      row-gap: 64px;
      grid-template-columns: 1fr;
      @media (min-width: 500px) {
        grid-template-columns: repeat(2, 1fr);
      }
      @media (min-width: 750px) {
        grid-template-columns: repeat(2, 1fr);
      }
      @media (min-width: 1100px) {
        grid-template-columns: repeat(4, 1fr);
      }

      .movie {
        position: relative;
        display: flex;
        flex-direction: column;

        .movie-video {
          position: relative;
          overflow: hidden;

          &:hover {
            .overview {
              transform: translateY(0);
            }
          }

          video {
            display: block;
            width: 100%;
            height: 100%;
          }

          .review {
            position: absolute;
            top: 0;
            left: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            width: 40px;
            height: 40px;
            background-color: #008f39;
            color: #fff;
            border-radius: 0 0 16px 0;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
              0 2px 4px -1px rgba(0, 0, 0, 0.06);
          }

          .overview {
            line-height: 1.5;
            position: absolute;
            bottom: 0;
            background-color: #228b22;
            padding: 12px;
            color: #fff;
            transform: translateY(100%);
            transition: 0.3s ease-in-out all;
          }
        }

        .info {
          margin-top: auto;
          .title {
            margin-top: 8px;
            color: #fff;
            font-size: 20px;
          }

          .release {
            margin-top: 8px;
            color: #c9c9c9;
          }

          .button {
            margin-top: 8px;
          }
        }
      }
    }
  }
}
</style>


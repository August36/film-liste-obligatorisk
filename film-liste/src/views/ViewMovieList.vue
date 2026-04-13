<script>
import MovieCard from '@/components/MovieCard.vue'

const TMDB_API_KEY = import.meta.env.VITE_TMDB_API_KEY
const TMDB_BASE_URL = 'https://api.themoviedb.org/3'

export default {
  name: 'ViewMovieList',
  components: {
    MovieCard,
  },
  emits: ['open-movie'],
  data() {
    return {
      movies: [],
      loading: false,
      error: null,
    }
  },
  methods: {
    handleOpenMovie(movie) {
      this.$emit('open-movie', movie)
    },
  },
  async mounted() {
    this.loading = true

    try {
      const movieIds = [  
      8363,
      30497,
      122,  
      747,  
      170,
    ]

      const moviesWithDetails = await Promise.all(
        movieIds.map(async (id) => {
          const res = await fetch(
            `${TMDB_BASE_URL}/movie/${id}?api_key=${TMDB_API_KEY}&language=en-US`,
          )

          if (!res.ok) throw new Error(`HTTP ${res.status}`)

          const movie = await res.json()

          return {
            id: movie.id,
            title: movie.title,
            poster_path: movie.poster_path,
            release_date: movie.release_date,
            overview: movie.overview,
            imdb_id: movie.imdb_id,
            likes: 0,
            comments: [],
            commentInput: '',
            showComments: false,
            seen: false,
          }
        }),
      )

      this.movies = moviesWithDetails
    } catch (e) {
      this.error = e.message
    } finally {
      this.loading = false
    }
  },
}
</script>

<template>
  <section class="movie-list">
    <h1>Movie List</h1>

    <p v-if="loading">Loading...</p>
    <p v-else-if="error">Fejl: {{ error }}</p>

    <ul v-else class="grid">
      <li v-for="movie in movies" :key="movie.id">
        <MovieCard :movie="movie" @open-movie="$emit('open-movie', $event)" />
      </li>
    </ul>
  </section>
</template>

<style scoped>


.movie-list {
  display: flex;
  flex-direction: column;
  padding: 1rem;
  align-items: center;
}

.movie-list > h1 {
  margin-bottom: 1rem;
}

.grid {
  width: 100%;
  list-style: none;
  padding: 0;
  margin: 0;
  display: grid;
  gap: 1rem;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
}

li {
  list-style: none;
}
</style>
<script>
const IMG_BASE = 'https://image.tmdb.org/t/p/w300'

export default {
  name: 'MovieCard',
  props: {
    movie: {
      type: Object,
      required: true,
    },
  },
  emits: ['open-movie'],
  methods: {
    posterUrl(path) {
      return path ? `${IMG_BASE}${path}` : ''
    },
    handleClick() {
      console.log('MovieCard clicked', this.movie)
      this.$emit('open-movie', this.movie)
    },
  },
  computed: {
    releaseYear() {
      return this.movie.release_date
        ? this.movie.release_date.slice(0, 4)
        : 'Ukendt år'
    },
  },
}
</script>

<template>
  <article class="card" @click="handleClick">
    <img
      v-if="movie.poster_path"
      :src="posterUrl(movie.poster_path)"
      :alt="movie.title"
    />

    <div class="movie-info">
      <h2>{{ movie.title }}</h2>
      <p>{{ releaseYear }}</p>
    </div>
  </article>
</template>

<style scoped>
.card {
  /* box-shadow: 2px 5px 6px #888888; */
  background-color: var(--color-main);
  border-radius: 20px;
  overflow: hidden;
  padding: 1rem;
  cursor: pointer;
  max-width: none;
  width: 100%;
  height: 550px;
}

.card img {
  display: block;
  width: 100%;
  max-width: 260px;
  margin: 0 auto;
  border-radius: 12px;
}

.movie-info {
  margin-top: 1rem;
}
</style>
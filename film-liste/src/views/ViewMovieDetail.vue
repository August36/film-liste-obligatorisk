<script>
import BaseButton from '@/components/BaseButton.vue';

const IMG_BASE = 'https://image.tmdb.org/t/p/w300'

export default {
  name: 'ViewMovieDetail',
    components: {
    BaseButton,
  },
  props: {
    movie: {
      type: Object,
      required: true,
    },
  },
  emits: ['go-back'],
  computed: {
    releaseYear() {
      return this.movie.release_date
        ? this.movie.release_date.slice(0, 4)
        : 'Ukendt år'
    },
    likeClass() {
      if (this.movie.likes <= 5) return 'likes-gray'
      if (this.movie.likes <= 10) return 'likes-blue'
      return 'likes-green'
    },
    seenText() {
      return this.movie.seen ? 'Du har set filmen' : 'Du har ikke set filmen'
    },
    commentButtonText() {
      return this.movie.showComments ? 'Skjul kommentarer' : 'Vis kommentarer'
    },
  },
  methods: {
    posterUrl(path) {
      return path ? `${IMG_BASE}${path}` : ''
    },
    addLike() {
      this.movie.likes++
    },
    toggleComments() {
      this.movie.showComments = !this.movie.showComments
    },
    addComment() {
      const trimmedComment = this.movie.commentInput.trim()

      if (!trimmedComment) return

      this.movie.comments.push(trimmedComment)
      this.movie.commentInput = ''
    },
    clearInput() {
      this.movie.commentInput = ''
    },
  },
}
</script>

<template>
  <section v-if="movie" class="detail" :class="{ seen: movie.seen }">
    <BaseButton class="back-button" @click="$emit('go-back')">Tilbage</BaseButton>

    <h1>{{ movie.title }}</h1>
    <p>{{ releaseYear }}</p>

    <img
      v-if="movie.poster_path"
      :src="posterUrl(movie.poster_path)"
      :alt="movie.title"
    />

    <p class="overview">{{ movie.overview }}</p>

    <a
      v-if="movie.imdb_id"
      :href="`https://www.imdb.com/title/${movie.imdb_id}/`"
      target="_blank"
      rel="noopener noreferrer"
    >
      Gå til IMDb
    </a>

    <div class="likes-section">
      <p :class="likeClass">Likes: {{ movie.likes }}</p>
      <BaseButton @click="addLike">Like</BaseButton>
    </div>

    <div class="seen-section">
      <label>
        <input v-model="movie.seen" type="checkbox" />
        Set
      </label>
      <p>{{ seenText }}</p>
    </div>

    <div class="comments-section">
      <BaseButton @click="toggleComments">{{ commentButtonText }}</BaseButton>

      <ul v-if="movie.showComments">
        <li v-if="movie.comments.length === 0">Ingen kommentarer endnu</li>
        <li v-for="(comment, index) in movie.comments" :key="index">
          {{ comment }}
        </li>
      </ul>

      <form class="comment-form" @submit.prevent="addComment">
        <label for="commentInput">Skriv en kommentar</label>
        <textarea
          id="commentInput"
          v-model="movie.commentInput"
          @keydown.enter.prevent="addComment"
        ></textarea>

        <div class="button-row">
          <BaseButton type="submit">Post kommentar</BaseButton>
          <BaseButton type="button" @click="clearInput">Slet alt</BaseButton>
        </div>
      </form>
    </div>
  </section>
</template>

<style scoped>
.detail {
  padding: var(--spacing-med);
  background-color: var(--color-bg);
  color: var(--color-text);
  min-height: 100vh;
  max-width: 430px;
  margin: 0 auto;
}

.detail.seen {
  background-color: #24413f;
}

.back-button {
  margin-bottom: var(--spacing-med);
}

.detail h1 {
  margin: 0 0 0.25rem 0;
  font-size: 2rem;
  line-height: 1.1;
}

.detail p {
  margin-top: 0;
}

.detail img {
  display: block;
  width: 100%;
  max-width: 280px;
  margin: 1rem 0;
  border-radius: 18px;
  object-fit: cover;
}

.overview {
  max-width: 600px;
  line-height: 1.5;
  margin-bottom: 0.5rem;
}

.detail a {
  color: #b79cff;
  text-decoration: underline;
}

.likes-gray {
  color: #bdbdbd;
}

.likes-blue {
  color: #9fd3ff;
}

.likes-green {
  color: #9be3b1;
}

.likes-section,
.seen-section,
.comments-section {
  margin-top: 1.25rem;
}

.likes-section {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 0.6rem;
}

.seen-section label {
  display: flex;
  align-items: center;
  gap: 0.45rem;
  margin-bottom: 0.35rem;
}

.seen-section p {
  margin: 0;
}

.comments-section ul {
  margin: 1rem 0 0 0;
  padding-left: 1.2rem;
}

.comments-section li {
  margin-bottom: 0.5rem;
}

.comment-form {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  margin-top: 1rem;
  max-width: 500px;
}

.comment-form label {
  font-weight: 600;
}

textarea {
  width: 100%;
  min-height: 120px;
  resize: vertical;
  padding: 0.9rem;
  border-radius: 14px;
  border: 1px solid rgba(255, 255, 255, 0.08);
  background-color: #f3f3f3;
  color: #111;
  font: inherit;
  box-sizing: border-box;
}

.button-row {
  display: flex;
  flex-wrap: wrap;
  gap: 0.75rem;
}
</style>
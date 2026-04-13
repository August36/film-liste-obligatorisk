<script>
import AppHeader from './components/AppHeader.vue'
import AppFooter from './components/AppFooter.vue'
import ViewLandingpage from './views/ViewLandingpage.vue'
import ViewMovieList from './views/ViewMovieList.vue'
import ViewMovieDetail from './views/ViewMovieDetail.vue'

export default {
  name: 'App',
  components: {
    AppHeader,
    AppFooter,
    ViewLandingpage,
    ViewMovieList,
    ViewMovieDetail,
  },
  data() {
    return {
      currentView: 'home',
      selectedMovie: null,
    }
  },
  methods: {
    goTo(view) {
      this.currentView = view
    },
    openMovieDetail(movie) {
      this.selectedMovie = movie
      this.currentView = 'detail'
    },
  },
}
</script>

<template>
  <div class="app">
  <header>
    <!-- AppHeader emitter eventet "navigate".
        Når det event modtages her, kaldes goTo-metoden. -->
    <AppHeader @navigate="goTo" />
  </header>

    <main>
      <ViewLandingpage
        v-if="currentView === 'home'"
        @go-to-movies="goTo('movies')"
      />

      <ViewMovieList
        v-else-if="currentView === 'movies'"
        @open-movie="openMovieDetail"
      />

      <ViewMovieDetail
        v-else-if="currentView === 'detail'"
        :movie="selectedMovie"
        @go-back="goTo('movies')"
      />
    </main>

    <footer>
      <AppFooter />
    </footer>
  </div>
</template>

<style scoped>
.app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

main {
  flex: 1;
}
</style>
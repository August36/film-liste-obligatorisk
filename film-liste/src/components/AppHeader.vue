<script>
import BaseButton from './BaseButton.vue';

export default {
  name: 'AppHeader',
  emits: ['navigate'],
  components: {
    BaseButton,
  },
  data() {
    return {
    }
  },
  computed: {
  },
  methods: {
  },
  watch: {
  },
}
</script>

<template>
    <nav>
        <!-- <div><img class="burgermenu-icon" src="../imgs/burger-menu-white.svg" alt=""></div> -->
        <BaseButton @click="$emit('navigate', 'home')">Home</BaseButton>
        <BaseButton @click="$emit('navigate', 'movies')">Filmliste</BaseButton>
    </nav>
</template>

<style scoped>

nav{
background-color: var(--color-secondary);
color: var(--color-text);
padding: var(--spacing-small);
display: flex;
justify-content: space-between;
height: 100%;
}
</style>
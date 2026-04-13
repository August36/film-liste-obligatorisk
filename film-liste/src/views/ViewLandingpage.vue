<script>
import BaseButton from '@/components/BaseButton.vue';

export default {
  name: 'ViewLandingpage',
  emits: ['go-to-movies'],
  components: {
    BaseButton,
  }
}
</script>

<template>
  <section class="landing">
    <h1>Velkommen</h1>
    <img class="landing-image" src="../imgs/cinema.jpg" alt="Biograf" />
    <BaseButton @click="$emit('go-to-movies')">Gå til movie list</BaseButton>
  </section>
</template>

<style scoped>
.landing {
  padding: 2rem 1.5rem;
  background-color: var(--color-bg);
  color: var(--color-text);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 1.25rem;
  text-align: center;
}

.landing-image {
  width: 100%;
  max-width: 320px;
  height: auto;
  display: block;
  border-radius: 16px;
  object-fit: cover;
  box-shadow: 0 10px 24px rgba(0, 0, 0, 0.3);
}
</style>
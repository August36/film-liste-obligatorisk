<template>
  <button class="base-button" @click="$emit('click')">
    <slot />
  </button>
</template>

<script>
export default {
  name: 'BaseButton',
  emits: ['click']
}
</script>

<style scoped>
.base-button {
  background-color: var(--color-button);
  color: var(--color-button-text);
  border: none;
  border-radius: 8px;
  padding: 0.75rem 1.25rem;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

.base-button:hover {
  background-color: var(--color-button-hover);
}

.base-button:focus-visible {
  outline: 2px solid var(--color-accent);
  outline-offset: 2px;
}
</style>
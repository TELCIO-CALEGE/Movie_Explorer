<script setup>
import MovieCard from './MovieCard.vue'
import { useRoute } from 'vue-router'

const route = useRoute()
defineProps({ filmes: { type: Array, required: true } })
</script>

<template>
  <section class="lista-filmes">
    <RouterLink
      v-for="filme in filmes"
      :key="filme.id"
      :to="{ path: '/filme/' + filme.id, query: route.query }"
      class="link-filme"
    >
      <MovieCard
        :titulo="filme.titulo"
        :ano="filme.ano"
        :classificacao="filme.classificacao"
        :imagem="filme.imagem"
        :realizador="filme.realizador"
      />
    </RouterLink>
  </section>
</template>

<style scoped>
.lista-filmes { display: grid; grid-template-columns: repeat(auto-fill, minmax(205px, 1fr)); gap: 24px; padding: 18px 0; }
.link-filme { text-decoration: none; color: inherit; border-radius: 12px; }
.link-filme:focus-visible { outline: 3px solid #2563eb; outline-offset: 4px; }
</style>

<script setup>
defineProps({
  titulo: String,
  ano: Number,
  classificacao: Number,
  imagem: String,
  realizador: String
})
</script>

<template>
  <article class="filme">
    <img v-if="imagem" :src="imagem" :alt="'Cartaz de ' + titulo" loading="lazy">
    <div v-else class="sem-imagem">Sem imagem</div>
    <div class="conteudo">
      <h3>{{ titulo }}</h3>
      <p>{{ ano }} · {{ realizador }}</p>
      <p class="nota">⭐ {{ classificacao.toFixed(1) }} / 10</p>
    </div>
  </article>
</template>

<style scoped>
.filme { overflow: hidden; border-radius: 12px; background: white; color: #17243a; box-shadow: 0 3px 12px #0f172a18; height: 100%; transition: transform .15s; }
.filme:hover { transform: translateY(-3px); }
.filme img, .sem-imagem { width: 100%; height: 300px; object-fit: cover; display: block; background: #e2e8f0; }
.sem-imagem { display: grid; place-items: center; }
.conteudo { padding: 13px 15px 18px; }
h3 { font-size: 17px; font-weight: 700; margin: 0 0 8px; }
p { color: #475569; font-size: 13px; margin: 6px 0; }
.nota { font-weight: 700; color: #1d4ed8; }
</style>

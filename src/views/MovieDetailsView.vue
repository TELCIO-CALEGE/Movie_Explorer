<script setup>
import { ref, watch, onUnmounted } from 'vue'
import { useRoute } from 'vue-router'
import { obterDetalhesFilme } from '../services/movieService.js'
const route = useRoute()
const filme = ref(null)
const carregando = ref(true)
const erro = ref('')
let numeroPedido = 0
async function carregarDetalhes() {
  const pedido = ++numeroPedido
  carregando.value = true
  erro.value = ''
  filme.value = null
  try {
    const dados = await obterDetalhesFilme(route.params.id)
    if (pedido === numeroPedido) filme.value = dados
  } catch(e) {
    if (pedido === numeroPedido) erro.value = e.message
  } finally {
    if (pedido === numeroPedido) carregando.value = false
  }
}
watch(() => route.params.id, carregarDetalhes, { immediate:true })
onUnmounted(() => { numeroPedido++ })
</script>
<template>
  <main>
    <RouterLink :to="{ path:'/', query:route.query }" class="voltar">← Voltar aos filmes</RouterLink>
    <p v-if="carregando">A carregar detalhes...</p>
    <p v-else-if="erro" role="alert">{{ erro }}</p>
    <article v-else-if="filme" class="detalhes">
      <img v-if="filme.imagem" :src="filme.imagem" :alt="'Cartaz de '+filme.titulo" class="cartaz" />
      <section class="informacoes">
        <h1>{{ filme.titulo }}</h1>
        <p><strong>Título original:</strong> {{ filme.tituloOriginal || 'Não disponível' }}</p>
        <p><strong>Ano:</strong> {{ filme.ano }}</p>
        <p><strong>Realizador:</strong> {{ filme.realizador }}</p>
        <p><strong>Produtor:</strong> {{ filme.produtor }}</p>
        <p><strong>Classificação:</strong> ⭐ {{ filme.classificacao.toFixed(1) }} / 10</p>
        <p><strong>Duração:</strong> {{ filme.duracao ? filme.duracao + ' minutos' : 'Não disponível' }}</p>
        <h2>Sinopse</h2>
        <p class="sinopse">{{ filme.descricao }}</p>
        <small>Sinopse fornecida em inglês pela API.</small>
      </section>
    </article>
  </main>
</template>
<style scoped>
main { max-width:1050px; margin:auto; padding:38px 22px; }
.voltar { display:inline-block; margin-bottom:28px; color:#2563eb; font-weight:700; text-decoration:none; }
.detalhes { display:flex; gap:35px; align-items:flex-start; }
.cartaz { width:290px; max-width:100%; border-radius:12px; box-shadow:0 8px 22px #0f172a24; }
.informacoes { flex:1; min-width:0; }
h1 { color:#2563eb; font-size:32px; margin:0 0 22px; }
h2 { margin-top:25px; }
p { line-height:1.7; margin:9px 0; }
.sinopse { white-space:pre-line; }
small { color:#64748b; }
@media(max-width:700px) { .detalhes { flex-direction:column; align-items:center; } .cartaz { width:260px; } .informacoes { width:100%; } }
</style>

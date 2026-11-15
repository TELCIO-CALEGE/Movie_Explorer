<script setup>
import { ref, watch, onUnmounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import MovieList from '../components/MovieList.vue'
import SearchBar from '../components/SearchBar.vue'
import MovieFilters from '../components/MovieFilters.vue'
import { obterCatalogo } from '../services/movieService.js'
const route = useRoute()
const router = useRouter()
const pesquisa = ref(String(route.query.pesquisa || ''))
const realizador = ref(String(route.query.realizador || ''))
const ano = ref(String(route.query.ano || ''))
const classificacao = ref(String(route.query.classificacao || ''))
const pagina = ref(Math.max(1, Number(route.query.pagina) || 1))
const filmes = ref([])
const realizadores = ref([])
const anos = ref([])
const totalResultados = ref(0)
const totalPaginas = ref(1)
const carregando = ref(false)
const erro = ref('')
let temporizador
let numeroPedido = 0

function guardarPesquisa() {
  const query = {}
  if (pesquisa.value.trim()) query.pesquisa = pesquisa.value.trim()
  if (realizador.value) query.realizador = realizador.value
  if (ano.value) query.ano = ano.value
  if (classificacao.value) query.classificacao = classificacao.value
  if (pagina.value > 1) query.pagina = String(pagina.value)
  router.replace({ path:'/', query })
}
async function carregarFilmes() {
  const pedido = ++numeroPedido
  carregando.value = true
  erro.value = ''
  try {
    const catalogo = await obterCatalogo()
    if (pedido !== numeroPedido) return
    realizadores.value = [...new Set(catalogo.map(f => f.realizador))].sort()
    anos.value = [...new Set(catalogo.map(f => f.ano).filter(Boolean))].sort((a,b) => b-a)
    const texto = pesquisa.value.trim().toLocaleLowerCase()
    const encontrados = catalogo.filter(f => (
      (!texto || f.titulo.toLocaleLowerCase().includes(texto)) &&
      (!realizador.value || f.realizador === realizador.value) &&
      (!ano.value || String(f.ano) === ano.value) &&
      (!classificacao.value || f.classificacao >= Number(classificacao.value))
    ))
    totalResultados.value = encontrados.length
    totalPaginas.value = Math.max(1, Math.ceil(encontrados.length / 12))
    if (pagina.value > totalPaginas.value) pagina.value = totalPaginas.value
    const inicio = (pagina.value - 1) * 12
    filmes.value = encontrados.slice(inicio, inicio + 12)
  } catch(e) {
    if (pedido === numeroPedido) {
      erro.value = e.message || 'Erro ao carregar filmes.'
      filmes.value = []
      totalResultados.value = 0
    }
  } finally {
    if (pedido === numeroPedido) carregando.value = false
  }
}
function mudarPagina(nova) {
  if (nova < 1 || nova > totalPaginas.value) return
  pagina.value = nova
  window.scrollTo({ top:0, behavior:'smooth' })
}
watch([pesquisa,realizador,ano,classificacao], () => { pagina.value = 1 })
watch([pesquisa,realizador,ano,classificacao,pagina], () => {
  clearTimeout(temporizador)
  numeroPedido++
  carregando.value = true
  temporizador = setTimeout(() => { guardarPesquisa(); carregarFilmes() }, 200)
}, { immediate:true })
onUnmounted(() => { clearTimeout(temporizador); numeroPedido++ })
</script>
<template>
  <main>
    <header>
      <small>Programação Web II · Vue.js</small>
      <h1>Movie Explorer</h1>
      <p>Explora, pesquisa e descobre filmes do Studio Ghibli.</p>
    </header>
    <SearchBar v-model="pesquisa" />
    <MovieFilters v-model:realizador="realizador" v-model:ano="ano"
      v-model:classificacao="classificacao" :realizadores="realizadores" :anos="anos" />
    <p v-if="carregando" class="mensagem">A carregar filmes...</p>
    <div v-else-if="erro" class="mensagem" role="alert">
      <p>{{ erro }}</p><button @click="carregarFilmes">Tentar novamente</button>
    </div>
    <template v-else>
      <p class="resultados">{{ totalResultados }} filme(s) encontrado(s)</p>
      <MovieList v-if="filmes.length" :filmes="filmes" />
      <p v-else class="mensagem">Nenhum filme encontrado. Experimenta outros filtros.</p>
      <nav v-if="totalPaginas > 1" class="paginacao">
        <button :disabled="pagina === 1" @click="mudarPagina(pagina-1)">← Anterior</button>
        <span>Página {{ pagina }} de {{ totalPaginas }}</span>
        <button :disabled="pagina === totalPaginas" @click="mudarPagina(pagina+1)">Próxima →</button>
      </nav>
    </template>
    <footer>Dados da API pública Studio Ghibli · Sem chave de API</footer>
  </main>
</template>
<style scoped>
main { max-width:1160px; margin:auto; padding:35px 22px 55px; }
header { text-align:center; }
header small, footer { color:#64748b; }
h1 { color:#2563eb; font-size:clamp(30px,5vw,42px); margin:8px 0; }
header p, .resultados { color:#475569; }
.resultados, .mensagem { text-align:center; }
.mensagem { margin:32px auto; }
.paginacao { display:flex; align-items:center; justify-content:center; gap:15px; margin:30px 0; }
button { border:0; border-radius:8px; background:#2563eb; color:white; padding:11px 16px; cursor:pointer; }
button:disabled { background:#94a3b8; cursor:not-allowed; }
footer { margin-top:45px; font-size:12px; text-align:center; }
</style>

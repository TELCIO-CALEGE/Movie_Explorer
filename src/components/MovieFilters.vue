<script setup>
defineProps({
  realizador: String, ano: String, classificacao: String,
  realizadores: { type: Array, default: () => [] },
  anos: { type: Array, default: () => [] }
})
const emit = defineEmits(['update:realizador','update:ano','update:classificacao'])
</script>
<template>
  <div class="filtros">
    <label>Realizador
      <select :value="realizador" @change="emit('update:realizador',$event.target.value)">
        <option value="">Todos os realizadores</option>
        <option v-for="nome in realizadores" :key="nome" :value="nome">{{ nome }}</option>
      </select>
    </label>
    <label>Ano
      <select :value="ano" @change="emit('update:ano',$event.target.value)">
        <option value="">Todos os anos</option>
        <option v-for="valor in anos" :key="valor" :value="String(valor)">{{ valor }}</option>
      </select>
    </label>
    <label>Classificação
      <select :value="classificacao" @change="emit('update:classificacao',$event.target.value)">
        <option value="">Todas as classificações</option>
        <option value="7">7 ou superior</option>
        <option value="8">8 ou superior</option>
        <option value="9">9 ou superior</option>
        <option value="9.5">9.5 ou superior</option>
      </select>
    </label>
  </div>
</template>
<style scoped>
.filtros { display:flex; flex-wrap:wrap; justify-content:center; gap:14px; margin:20px 0 30px; }
label { display:grid; gap:5px; text-align:left; font-size:14px; font-weight:600; }
select { min-width:170px; max-width:280px; padding:11px; border:1px solid #cbd5e1; border-radius:8px; background:white; color:#17243a; }
</style>

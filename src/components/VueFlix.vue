<script setup>
import { ref, computed } from 'vue';

// Função que mostra o form de filme.
const mostrarFilmeForm = ref(false);

// Array que reúne os filmes.
const catalogoFilmes = ref([]);

// Função que adiciona o filme.
// Inputs
const inputNomeFilme = ref("");
const inputImgFilme = ref("");
const inputAnoFilme = ref("");
const inputGeneroFilme = ref("");

// Função que adiciona um filme ao catálogo.
const adicionarFilme = () => {
  const vazio = ref("");
  const generoExistente = ref("");

  switch (vazio.value) {
    case inputNomeFilme.value:
      alert("É necessário um título para o filme!");
      break;

    case inputImgFilme.value:
      alert("É necessário uma imagem para o filme!");
      break;

    case inputAnoFilme.value:
      alert("É necessário um ano para o filme!");
      break;

    case inputGeneroFilme.value:
      alert("É necessário um gênero para o filme!");
      break;

    default:
      catalogoFilmes.value.push({
        nomeFilme: inputNomeFilme.value,
        imgFilme: inputImgFilme.value,
        anoFilme: inputAnoFilme.value,
        generoFilme: inputGeneroFilme.value,
        avaliacaoFilme: false,
      });

      // Verificar se o gênero já está na lista de filtros
      generoExistente.value = filtrosGeneros.value.find(genero => genero.nomeGenero === inputGeneroFilme.value);
      if (!generoExistente.value) {
        filtrosGeneros.value.push({ nomeGenero: inputGeneroFilme.value });
      }

      limpaCampos();
  }
};

// Função que limpa os campos de inclusão de filme.
const limpaCampos = () => {
  inputNomeFilme.value = "";
  inputImgFilme.value = "";
  inputAnoFilme.value = "";
  inputGeneroFilme.value = "";
  mostrarFilmeForm.value = false;
};

// Função que exclui o filme do catálogo.
const excluirFilme = (index) => {
  const generoParaRemover = catalogoFilmes.value[index].generoFilme;
  catalogoFilmes.value.splice(index, 1);

  // Verificar se o gênero ainda existe em outros filmes.
  const aindaExisteGenero = catalogoFilmes.value.some(filme => filme.generoFilme === generoParaRemover);

  if (!aindaExisteGenero) {
    filtrosGeneros.value = filtrosGeneros.value.filter(genero => genero.nomeGenero !== generoParaRemover);
  }
};

// Função que avalia o filme.
const avaliarFilme = (index, avaliacaoFilme) => {
  catalogoFilmes.value[index].avaliacaoFilme = avaliacaoFilme;
};

// FILTROS //
// Filtrar por gênero.
const filtrosGeneros = ref([]);
const filtroGeneroAtual = ref("Todos");

// Função
const filmesFiltrados = computed(() => {
  if (filtroGeneroAtual.value === 'Todos') {
    return catalogoFilmes.value;
  }
  return catalogoFilmes.value.filter(filme => filme.generoFilme === filtroGeneroAtual.value);
});

</script>

<template>
  <div class="vueflix">
    <div class="acoes-usuario">
      <div class="filtros">
        <div class="titulo">Filtrar</div>
        <div class="opcoes-filtros">
          <button class="botao" :class="{ativo: filtroGeneroAtual === 'Todos'}" @click="filtroGeneroAtual = 'Todos'">Todos</button>
          <!-- Adiciona os botões de filtro para cada gênero -->
          <button v-for="genero in filtrosGeneros" :key="genero.nomeGenero" class="botao" 
          :class="{ativo: filtroGeneroAtual === genero.nomeGenero}" @click="filtroGeneroAtual = genero.nomeGenero">
            {{ genero.nomeGenero }}
          </button>
        </div>
      </div>

      <div class="novo-filme">
        <div v-if="mostrarFilmeForm" class="adicionar-filme">
          <input v-model="inputNomeFilme" type="text" autocomplete="off" placeholder="Nome do Filme" required />
          <input v-model="inputImgFilme" type="text" autocomplete="off" placeholder="URL da Imagem" required />
          <input v-model="inputAnoFilme" type="text" autocomplete="off" placeholder="Ano de Lançamento" required />
          <input v-model="inputGeneroFilme" type="text" autocomplete="off" placeholder="Gênero" required />
          <div class="acoes">
            <button class="botao ativo" @click="adicionarFilme">Salvar</button>
            <button class="botao danger ativo" @click="limpaCampos">Cancelar</button>
          </div>
        </div>
        <button v-else class="botao ativo" @click="mostrarFilmeForm = true">Adicionar Filme</button>
      </div>
    </div>

    <div class="filmes">
      <div v-for="(filme, index) in filmesFiltrados" :key="index" class="filme">
        <div class="capa-container">
          <div class="acoes-filme">
            <button class="botao" @click="avaliarFilme(index, true)" :class="{ativo: filme.avaliacaoFilme === true}">Gostei</button>
            <button class="botao danger" @click="avaliarFilme(index, false)" :class="{ativo: filme.avaliacaoFilme === false}">Não Gostei</button>
            <button class="botao danger" @click="excluirFilme(index)">Excluir</button>
          </div>
          <img class="capa" :src="filme.imgFilme" alt="" />
        </div>
        <div class="nome">{{ filme.nomeFilme }}</div>
        <div class="info">{{ filme.anoFilme }} - {{ filme.generoFilme }}</div>
      </div>
    </div>
  </div>
</template>

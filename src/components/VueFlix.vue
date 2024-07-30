<script setup>
import { ref } from 'vue';

// Função que mostra o form de filme.
const mostrarFilmeForm = ref(false);

// Array que reune os filmes.
const catalogoFilmes = ref([
  {
    anoFilme: '2024',
    generoFilme: 'Ação',
    avaliacaoFilme: true,
    nomeFilme: 'morto piscina e logan',
    imgFilme: 'https://media.themoviedb.org/t/p/w300_and_h450_bestv2/8cdWjvZQUExUUTzyp4t6EDMubfO.jpg',
  },
  {
    anoFilme: '2024',
    generoFilme: 'Ação',
    avaliacaoFilme: false,
    nomeFilme: 'morto piscina e logan',
    imgFilme: 'https://media.themoviedb.org/t/p/w300_and_h450_bestv2/8cdWjvZQUExUUTzyp4t6EDMubfO.jpg',
  },
  {
    anoFilme: '2024',
    generoFilme: 'Ação',
    avaliacaoFilme: false,
    nomeFilme: 'morto piscina e logan',
    imgFilme: 'https://media.themoviedb.org/t/p/w300_and_h450_bestv2/8cdWjvZQUExUUTzyp4t6EDMubfO.jpg',
  },
]);

// Função que adiciona o filme.
// Inputs
const inputNomeFilme = ref("");
const inputImgFilme = ref("");
const inputAnoFilme = ref("");
const inputGeneroFilme = ref("");

// Função.
const adicionarFilme = () => {
  const vazio = "";

  switch (vazio) {
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

      limpaCampos();
  }
};
const cancelaConfirma = () => {
  if (dangerConfirm()) {
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
  if (dangerConfirm()) {
    catalogoFilmes.value.splice(index, 1);
  }
};

// Função que avalia o filme.
const avaliarFilme = (index, avaliacaoFilme) => {
  catalogoFilmes.value[index].avaliacaoFilme = avaliacaoFilme;
};

// Função de confirmação de ação perigosa.
const dangerConfirm = () => {
  return confirm("Você tem certeza que deseja fazer isso?");
};

</script>
<template>
  <div class="vueflix">
    <div class="acoes-usuario">
      <div class="filtros">
        <div class="titulo">Filtrar</div>
        <div class="opcoes-filtros">
          <button class="botao ativo">Todos</button>
          <!-- Filter buttons logic can be implemented here if needed -->
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
            <button class="botao danger ativo" @click="cancelaConfirma">Cancelar</button>
          </div>
        </div>
        <button v-else class="botao ativo" @click="mostrarFilmeForm = true">Adicionar Filme</button>
      </div>
    </div>

    <div class="filmes">
      <div v-for="(filme, index) in catalogoFilmes" :key="index" class="filme">
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

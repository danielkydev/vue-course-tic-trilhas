<script setup>
import { ref } from 'vue';
// Função que mostra o form de filme.
const mostrarFilmeForm = ref(false);

// Array que reune os filmes.
const catalogoFilmes = ref([
  {
    nomeFilme: 'morto piscina e logan',
    imgFilme: 'https://media.themoviedb.org/t/p/w300_and_h450_bestv2/8cdWjvZQUExUUTzyp4t6EDMubfO.jpg',
    anoFilme: '2024',
    generoFilme: 'Ação',
  },
  {
    nomeFilme: '22222',
    imgFilme: 'https://media.themoviedb.org/t/p/w300_and_h450_bestv2/8cdWjvZQUExUUTzyp4t6EDMubfO.jpg',
    anoFilme: '2024',
    generoFilme: 'Ação',
  },
  {
    nomeFilme: '333333',
    imgFilme: 'https://media.themoviedb.org/t/p/w300_and_h450_bestv2/8cdWjvZQUExUUTzyp4t6EDMubfO.jpg',
    anoFilme: '2024',
    generoFilme: 'Ação',
  }
]);

// Função que adiciona o filme.
// Inputs
const inputNomeFilme = ref ("");
const inputImgFilme = ref ("");
const inputAnoFilme = ref ("");
const inputGeneroFilme = ref ("");

// Função.
const adicionarFilme = () => {
  const vazio = "";

  switch(vazio) {
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
  });

  limpaCampos();
}};

// Função que limpa os campos de inclusão de filme.
const limpaCampos = () => {
  inputNomeFilme.value = "";
  inputImgFilme.value = "";
  inputAnoFilme.value = "";
  inputGeneroFilme.value = "";
  mostrarFilmeForm.value = false;
};

// Função

// Função que filtra os filmes.

// Função que avalia o filme.

</script>
<template>
  <div class="vueflix">
    <div class="acoes-usuario">
      <div class="filtros">
        <div class="titulo">Filtrar</div>
        <div class="opcoes-filtros">
          <button class="botao ativo">Todos</button>
          <button class="botao">Gostei</button>
          <button class="botao">Não Gostei</button>
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
            <button class="botao danger ativo" @click="limpaCampos" >Cancelar</button>
          </div>
        </div>
        <button v-else class="botao ativo" @click="mostrarFilmeForm = true">Adicionar Filme</button>
      </div>
    </div>

    <div class="filmes">
      <div v-for="filme in catalogoFilmes" :key="filme.id" class="filme">
        <div class="capa-container">
          <div class="acoes-filme">
            <button class="botao">Gostei</button>
            <button class="botao danger">Não Gostei</button>
            <button class="botao danger">Excluir</button>
          </div>
          <img class="capa" :src="filme.imgFilme" alt="" />
        </div>
        <div class="nome">{{ filme.nomeFilme }}</div>
        <div class="info">{{filme.anoFilme}} - {{ filme.generoFilme }}</div>
      </div>
    </div>
  </div>
</template>

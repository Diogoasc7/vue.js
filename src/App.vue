<script setup>
import { reactive } from 'vue';

const nome = 'Diogo Costa'
const meuObj = {
  nome: 'Diogo',
  filmeFavorito: 'Anjos da Lei'
}

function dizOi(nome) {
  return `${nome}, diz oi`;
}

const enderecoDaImagemDoBatman = 'https://th.bing.com/th/id/OIP.ke8IDSQfwyNtqiHT6R3yLgHaEK?rs=1&pid=ImgDetMain';
const enderecoImagemDoSuperMan = 'https://th.bing.com/th/id/OIP.6c3aghwWvZXDWhDKbTqVeQHaF1?rs=1&pid=ImgDetMain';

const botaoEstaDesabilitado = false;

const gostaDoBatman = true;
const gostaDoSuperMan = false;

const estaAutorizado = false;

const estado = reactive({//O estado é a fonte central dos dados da aplicação. Qualquer alteração nesse objeto reativo reflete automaticamente na tela.
  contador: 0,
  email: '',
  saldo: 5000,
  transferindo: 0,
  nomes: ['gian', 'paulo', 'luisa', 'monica'],
  nomeAInserir: '',
})

function incrementar() {
  estado.contador++;
}

function decrementar() {
  estado.contador--;
}

function alteraEmail(evento) {
  estado.email = evento.target.value;
}

function mostraSaldoFuturo() {
  const {  saldo, transferindo} = estado;
  return saldo - transferindo;
}

function validaValorTransferencia() {
  const {  saldo, transferindo} = estado;
  return saldo >= transferindo;
}

function cadastrarNome() {
  if (estado.nomeAInserir.length >= 3) {
    estado.nomes.push(estado.nomeAInserir);
  } else {
    alert('Digite mais caracteres')
  }
  console.log(estado);
}
</script>

<template>
  <h1>{{ dizOi('Felipe') }}</h1>
  <img v-if="gostaDoBatman" :src="enderecoDaImagemDoBatman" alt="">
  <img v-else-if="gostaDoSuperMan" :src="enderecoImagemDoSuperMan" alt="">
  <h2 v-else>Não curte heróis da dc</h2>

  <h1 v-if="estaAutorizado">Bem vindo</h1>
  <h1 v-else>Não possui acesso</h1>

  <button :disabled='botaoEstaHabilitado'>enviar mensagem</button>

  <br>
  <hr>

  {{ estado.contador }}

  <button @click="incrementar" type="button">+</button>
  <button @click="decrementar" type="button">-</button>

  <br>
  <hr>

  {{ estado.email }}
  <input type="email" @keyup="alteraEmail">

  <br>
  <hr>

  Saldo: {{ estado.saldo }} <br>
  Transferindo: {{ estado.transferindo  }} <br>
  Saldo depois da transferência: {{ mostraSaldoFuturo() }} <br>
  <input class="campo" :class="{ invalido: !validaValorTransferencia() }" @keyup="evento => estado.transferindo = evento.target.value">
  <button v-if="validaValorTransferencia()">Transferir</button>
  <span v-else>Valor maior que o saldo</span>

  <br>
  <hr>

  <ul>
    <li v-for="nome in estado.nomes">
      {{ nome }}
    </li>
  </ul>
  <input @keyup="evento => estado.nomeAInserir = evento.target.value">
  <button @click="cadastrarNome" type="button">Cadastrar nome</button>
</template>

<style scoped>
  img {
    max-width: 200px;
  }

  .invalido {
    outline-color: red;
    border-color: red;
  }

  .campo {
    border: 2px solid black;
  }
</style>

<!--
Explicação por temas principais (sem exemplos)
Reatividade com reactive
No Vue.js, reatividade significa que a interface do usuário é automaticamente atualizada sempre que os dados mudam. O reactive cria um objeto observável, ou seja, o Vue monitora suas propriedades. Quando qualquer valor dentro desse objeto muda, o Vue atualiza automaticamente a parte da interface que depende desse valor.

Eventos
Eventos são formas de interagir com o usuário e responder a ações como cliques, digitação ou envio de formulários. No Vue, os eventos do DOM são escutados diretamente nos elementos HTML e vinculados a funções que alteram os dados reativos ou executam alguma lógica necessária.

Renderização Condicional
A renderização condicional permite que partes da interface sejam exibidas ou ocultadas com base em condições lógicas. Essa funcionalidade torna a aplicação mais dinâmica, pois ela mostra diferentes conteúdos dependendo do estado atual dos dados.

Estilos Condicionais
Os estilos condicionais controlam a aparência de elementos com base em variáveis ou expressões. Isso permite aplicar ou remover classes CSS dinamicamente, adaptando o visual da interface conforme o comportamento do usuário ou o estado da aplicação.

Listas com v-for
A renderização de listas permite exibir elementos repetidamente com base em estruturas de dados como arrays. O Vue percorre os itens de uma lista e gera dinamicamente os elementos correspondentes, mantendo tudo reativo — ou seja, se a lista mudar, a interface muda também.
-->
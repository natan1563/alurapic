

<template>
  <div>
    <h1 class="centralizado">{{ titulo}}</h1>

    <input type="search" class="filtro" v-on:input="filtro = $event.target.value" placeholder="Filtro por titulo">
    <ul class="lista-fotos">
      <li class="lista-fotos-item" v-for="foto of fotosComFiltros" :key="foto">

        <meu-painel :titulo="foto.titulo">
          <imagem-responsiva :url="foto.url" :titulo="foto.titulo"/>
          <meu-botao
            tipo="button"
            rotulo="REMOVER"
            @botaoAtivado="remove(foto)"
            :confirmacao="false"
            estilo="perigo"
            />
        </meu-painel>

      </li>
    </ul>
  </div>
</template>

<script>
import Painel from '../shared/painel/Painel.vue';
import ImagemResponsiva from '../shared/imagem-responsiva/imagemResponsiva.vue';
import Botao from '../shared/botao/Botao.vue';

export default {

  components: {
    'meu-painel': Painel,
    'imagem-responsiva': ImagemResponsiva,
    'meu-botao': Botao
  },

  data() {
    return {
      titulo: 'Alurapic',
      fotos: [],
      filtro: ''
    }
  },

  computed: {

    fotosComFiltros() {

      if (this.filtro) {
        let expressao = new RegExp(this.filtro.trim(), 'i');
        return this.fotos.filter(foto => expressao.test(foto.titulo));
      }

      return this.fotos;
    }
  },

  methods: {
    remove(foto) {
      alert('Remover a foto' + foto.titulo);
    }
  },

  created() {
    let promise = this.$http.get('http://localhost:3000/v1/fotos')
      .then(response => response.json())
      .then(fotos => this.fotos = fotos, error => console.log('Deu pau ai irmao'));

  }
}
</script>

<style>
 .centralizado {
    text-align: center;
  }

  .lista-fotos {
    list-style: none;
  }

  .lista-fotos .lista-fotos-item {
    display: inline-block;
  }



  .filtro {
    display: block;
    width: 100%;
  }
</style>

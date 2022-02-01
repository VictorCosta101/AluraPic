<template>
  <div class="corpo">
    <h1 class="centralizado">{{ titulo }}</h1>
    <input type="search" class="filtro" @input="filtro =$event.target.value" placeholder="filtre por parte do titulo">
    <ul class="lista-fotos">
      <li class="lista-fotos-item" v-for="foto in fotosComFiltro">
        <meu-painel :titulo="foto.titulo">
          <img :src="foto.url" :alt="foto.titulo" />
        </meu-painel>
      </li>
    </ul>
  
  </div>
</template>

<script>
import Painel from "./components/shared/painel/Painel.vue";

export default {
  components: {
    "meu-painel": Painel,
    
  },
  data() {
    return {
      titulo: "AluraPic",
      fotos: [],
      filtro:''
    };
  },

  computed: {
    fotosComFiltro(){
      if(this.filtro){
        let exp = new RegExp(this.filtro.trim(), 'i');
        return this.fotos.filter(foto => exp.test(foto.titulo));
      }else{
        return this.fotos;
      }
    }
  },
  // alurapic/src/App.vue
  // código anterior omitido

  created() {
    let promise = this.$http.get("http://localhost:3000/v1/fotos");
    promise
      .then((res) => res.json())
      .then(
        (fotos) => (this.fotos = fotos),
        (err) => console.log(err)
      );
  },

  // código posterior omitido
};
</script>

<style>
.corpo {
  font-family: Helvetica, sans-serif;
  width: 96%;
  margin: 0 auto;
}
.centralizado {
  text-align: center;
}
.lista-fotos {
  list-style: none;
}
.lista-fotos .lista-fotos-item {
  display: inline-block;
}
.filtro{
  display: block;
  width: 100%;
}
</style>

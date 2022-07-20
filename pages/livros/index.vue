<template>
  <v-container>
    <h1>Consulta de Livros</h1>
    <hr>
    <v-row style="margin-top: 2%">
      <v-col>
        <v-btn
          color="blue"
          outlined
          @click="getCategorias"
        >
          Pesquisar
          </v-btn>
      </v-col>
      <v-col
        cols="2"
      >
        <v-btn
          to="/livros/cadastro"
          color="green"
        >
          cadastro
        </v-btn>
      </v-col>
    </v-row>
    <v-container>
      <v-row>
      <v-col
      cols="max"
      >
      <v-data-table
        :headers="headers"
        :items="categorias"
        :items-per-page="10"
        class="elevation-1"
      ></v-data-table>
      </v-col>
    </v-row>
    </v-container>
  </v-container>
</template>

<script>
export default {
  name: 'ConsultaLivrosPage',

  data () {
    return{
      headers: [
        {
          text: 'Código',
          align: 'center',
          sortable: true,
          value: 'id',
        },
        {
          text: 'Titulo',
          align: 'center',
          sortable: true,
          value: 'titulo',
        },
        {
          text: 'Sinopse',
          align: 'center',
          sortable: true,
          value: 'sinopse',
        },
        {
          text: 'Categoria',
          align: 'center',
          sortable: true,
          value: 'categoria.nome',
        },
        {
          text: 'Autor',
          align: 'center',
          sortable: true,
          value: 'autor.nome',
        }
      ],
      categorias: []
    }
  },

  methods: {
    async getCategorias () {
      this.categorias = await this.$axios.$get(`http://localhost:3333/livro`)
    }
  }

}
</script>

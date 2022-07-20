<template>
  <v-container>
    <h1>Consulta de categorias</h1>
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
          to="/categorias/cadastro"
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
  name: 'ConsultaCategoriasPage',

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
          text: 'Nome',
          align: 'center',
          sortable: true,
          value: 'nome',
        }
      ],
      categorias: []
    }
  },

  methods: {
    async getCategorias () {
      this.categorias = await this.$axios.$get(`http://localhost:3333/categorias`)
    }
  }

}
</script>

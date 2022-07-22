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
         <v-icon>
          mdi-plus
         </v-icon> 
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
      >
        <template v-slot:item.actions="{ item }">
          <v-icon
            small
            class="mr-2"
            @click="editar(item)"
          >
            mdi-pencil
          </v-icon>
          <v-icon
            small
            @click="deletar(item)"
          >
            mdi-delete
          </v-icon>
        </template>
      </v-data-table>
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
        // {
        //   text: 'Sinopse',
        //   align: 'center',
        //   sortable: true,
        //   value: 'sinopse',
        // },
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
        },
        { text: "", value: "actions" }
      ],
      categorias: []
    }
  },

  methods: {
     getCategorias: async function () {
      console.log(await this.$axios.$get(`http://localhost:3333/livro`));
      this.categorias = await this.$axios.$get(`http://localhost:3333/livro`)
    },
    async deletar (autor) {
      try {
        if (confirm(`Deseja deletar o livro id ${autor.id} - ${autor.nome}?`)) {
          let response = await this.$axios.$post('http://localhost:3333/livro/deletar', { id: autor.id });
          this.$toast.success(response.message)
          this.getCategorias();
        }
      } catch (error) {
        this.$toast.error(error.message)
      }  
    },
    async editar (categoria) {
      this.$router.push({
        name: 'livros-cadastro',
        params: { id: categoria.id }
      });
    }
  },
  beforeMount(){
    this.getCategorias()
  }

}
</script>

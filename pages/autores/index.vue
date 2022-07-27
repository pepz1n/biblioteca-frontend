<template>
  <v-container>
    <h1>Consulta de Autores</h1>
    <hr>
    <v-row style="margin-top: 2%">
      <v-col>
        <v-btn
          color="blue"
          outlined
          @click="getAutores"
        >
          Pesquisar
          </v-btn>
      </v-col>
      <v-col
        cols="2"
      >
        <v-btn
          to="/autores/cadastro"
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
        :items="autores"
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
  name: 'ConsultaAutoresPage',

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
        },
        {
          text: 'Email',
          align: 'center',
          sortable: true,
          value: 'email',
        },
        { text: "", value: "actions" }
      ],
      autores: []
    }
  },

  methods: {
    getAutores: async function () {
      this.autores = await this.$axios.$get(`http://localhost:3333/autor`)
    },
    
    async deletar (autor) {
      try {
        if (confirm(`Deseja deletar o autor id ${autor.id} - ${autor.nome}?`)) {
          let response = await this.$axios.$post('http://localhost:3333/autor/deletar', { id: autor.id });
          this.$toast.success(response.message)
          this.getAutores();
        } 
      } catch (error) {
        this.$toast.error(error.message)
      }
    },
    async editar (autor) {
      this.$router.push({
        name: 'autores-cadastro',
        params: { id: autor.id }
      });
    }
  },
  created(){
    this.getAutores()
  }

}
</script>

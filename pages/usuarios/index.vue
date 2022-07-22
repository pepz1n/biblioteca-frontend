<template>
  <v-container>
    <h1>Consulta de usuario</h1>
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
          to="/usuarios/cadastro"
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
  name: 'ConsultaUsuariosPage',

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
          text: 'CPF/CNPJ',
          align: 'center',
          sortable: true,
          value: 'cpfcnpj',
        },
        {
          text: 'E-mail',
          align: 'center',
          sortable: true,
          value: 'email',
        },
        {
          text: 'Telefone',
          align: 'center',
          sortable: true,
          value: 'telefone',
        },
        { text: "", value: "actions" }
      ],
      categorias: []
    }
  },

  methods: {
    getCategorias: async function () {
        this.categorias = await this.$axios.$get(`http://localhost:3333/usuario`)
    },
    async deletar (autor) {
      try {
        if (confirm(`Deseja deletar o usuario id ${autor.id} - ${autor.nome}?`)) {
          let response = await this.$axios.$post('http://localhost:3333/usuario/deletar', { id: autor.id });
          this.$toast.success(response.message)
          this.getCategorias();
        }
      } catch (error) {
        this.$toast.error(error.message)
      }
    },
    async editar (categoria) {
      this.$router.push({
        name: 'usuarios-cadastro',
        params: { id: categoria.id }
      });
    }
  },
   beforeMount(){
      this.getCategorias()
    }
}
</script>

<template>
  <v-container>
    <h1>Consulta de usuario</h1>
    <hr>
    <v-row style="margin-top: 2%">
      <v-col>
        <v-btn
          color="blue"
          outlined
          @click="getUsuario"
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
        :items="usuario"
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
      usuario: []
    }
  },

  methods: {
    getUsuario: async function () {
        this.usuario = await this.$axios.$get(`http://localhost:3333/usuario`)
    },
    async deletar (usuarioDelete) {
      try {
        if (confirm(`Deseja deletar o usuario id ${usuarioDelete.id} - ${usuarioDelete.nome}?`)) {
          let response = await this.$axios.$post('http://localhost:3333/usuario/deletar', { id: usuarioDelete.id });
          this.$toast.success(response.message)
          this.getUsuario();
        }
      } catch (error) {
        this.$toast.error(error.message)
      }
    },
    async editar (usuarioEditar) {
      this.$router.push({
        name: 'usuarios-cadastro',
        params: { id: usuarioEditar.id }
      });
    }
  },
   beforeMount(){
      this.getUsuario()
    }
}
</script>

<template>
  <v-container>
    <h1 style="text-align: center">Cadastro de Categorias</h1>
    <hr>
    <v-form>
      <v-container>
        <v-row>
          <v-col>
            <v-text-field
              v-model="categoria.id"
              placeholder="codigo"
              label="codigo"
              disabled
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="categoria.nome"
              placeholder="Nome"
              label="Nome"
              outlined
            />
          </v-col>
        </v-row>
      </v-container>
 </v-form>
   <v-btn
      outlined
      @click="cadastrar"
      color="green"
    >
      cadastrar
    </v-btn>
    <v-btn
      outlined
      to="/categorias"
      color="red"
    >
      cancelar
    </v-btn>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroCategoriasPage',

  data () {
    return {
      categoria: {
        id: null,
        nome: null
      }
    }
  },

  methods: {
    async cadastrar () {
      let categoria = {
        nome: this.categoria.nome
      };
      let response = await this.$axios.$post('http://localhost:3333/categorias', categoria);
      console.log(response);
    }
  }
}
</script>
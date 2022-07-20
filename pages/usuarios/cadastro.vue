<template>
  <v-container>
    <h1  style="text-align: center">Cadastro de usuarios</h1>
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
        <v-row>
          <v-col>
            <v-text-field
              v-model="categoria.cpfcnpj"
              placeholder="CPF/CNPJ"
              label="CPF/CNPJ"
              outlined
            />
          </v-col>
        </v-row>
          <v-row>
          <v-col>
            <v-text-field
              v-model="categoria.email"
              placeholder="E-MAIL"
              label="E-MAIL"
              outlined
            />
          </v-col>
          <v-col>
            <v-text-field
              v-model="categoria.telefone"
              placeholder="Telefone"
              label="Telefone"
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
      to="/usuarios"
      color="red"
    >
      cancelar
    </v-btn>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroLivrosPage',

  data () {
    return {
      categoria: {
        nome: null,
        cpfcnpj: null,
        email: null,
        telefone: null,
      }
    }
  },

  methods: {
    async cadastrar () {
      let categoria = {
        nome: this.categoria.nome,
        cpfcnpj: this.categoria.cpfcnpj,
        email: this.categoria.email,
        telefone: this.categoria.telefone
      };
      let response = await this.$axios.$post('http://localhost:3333/usuario', categoria);
      console.log(response);
    }
  }
}
</script>
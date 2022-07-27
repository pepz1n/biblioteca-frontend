<template>
  <v-container>
    <h1  style="text-align: center">Cadastro de usuarios</h1>
    <hr>
    <v-form v-model="valid">
      <v-container>
        <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.id"
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
              v-model="usuario.nome"
              placeholder="Nome"
              label="Nome"
              :rules="rule"
              required
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.cpfcnpj"
              placeholder="CPF/CNPJ"
              label="CPF/CNPJ"
              :rules="rule"
              required
              outlined
            />
          </v-col>
        </v-row>
          <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.email"
              placeholder="E-MAIL"
              label="E-MAIL"
              :rules="rule"
              required
              outlined
            />
          </v-col>
          <v-col>
            <v-text-field
              v-model="usuario.telefone"
              placeholder="Telefone"
              label="Telefone"
              :rules="rule"
              required
              outlined
            />
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-btn
      outlined
      @click="persistir"
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
      valid: false,
      usuario: {
        nome: null,
        cpfcnpj: null,
        email: null,
        telefone: null,
      },
      rule: [
        v => !!v || 'Esse campo é obrigatório'
      ]
    }
  },
  created () {
    if (this.$route?.params?.id) {
      this.getById(this.$route.params.id)
    }
  },

  methods: {
    async persistir () {
      try { 
        if (!this.valid) {
          return this.$toast.warning('O formulário de cadastro não é válido!')
        }
        let usuario = {
        nome: this.usuario.nome,
        cpfcnpj: this.usuario.cpfcnpj,
        email: this.usuario.email,
        telefone: this.usuario.telefone
        };
        
        if(!this.usuario.id){  
          let response = await this.$axios.$post('http://localhost:3333/usuario', usuario);
          this.$router.push('/usuarios')
          return this.$toast.success(`${response.nome} cadastrado com sucesso`)
        }
        await this.$axios.$post(`http://localhost:3333/usuario/${this.usuario.id}`, usuario )
        this.$router.push('/usuarios')
        this.$toast.success('Cadastro atualizado com sucesso!');
      } catch (error) {
        this.$toast.error("DON't FORGET AMEM")
      }
    },
    async getById (id) {
      this.usuario = await this.$axios.$get(`http://localhost:3333/usuario/${id}`);
    }
  }
}
</script>
<template>
  <v-container>
    <h1 style="text-align: center">Cadastro de Autores</h1>
    <hr>
    <v-form v-model="valid">
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
              :rules="rule"
              required
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="categoria.email"
              placeholder="Email"
              label="Email"
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
      to="/autores"
      color="red"
    >
      cancelar
    </v-btn>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroAutoresPage',

  data () {
    return {
      valid: false,
      categoria: {
        id: null,
        nome: null,
        email:null
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
        let categoria = {
          nome: this.categoria.nome,
          email: this.categoria.email,
        };
        
        if(!this.categoria.id){
          let response = await this.$axios.$post('http://localhost:3333/autor', categoria);
          this.$router.push('/autores')
          return this.$toast.success(`${response.nome} cadastrado com sucesso`)
        }

        await this.$axios.$post(`http://localhost:3333/autor/${this.categoria.id}`, categoria )
        this.$router.push('/autores')
        this.$toast.success('Cadastro atualizado com sucesso!');
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!');
        
      }
    },
    async getById (id) {
      this.categoria = await this.$axios.$get(`http://localhost:3333/autor/${id}`);
    }
  }
}
</script>
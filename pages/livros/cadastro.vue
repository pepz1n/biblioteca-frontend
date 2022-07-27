<template>
  <v-container>
    <h1 style="text-align: center">Cadastro de Livros</h1>
    <hr>
    <v-form v-model="valid">
      <v-container>
        <v-row>
          <v-col>
            <v-text-field
              v-model="livro.id"
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
              v-model="livro.titulo"
              placeholder="Titulo"
              label="Titulo"
              :rules="rule"
              required
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-textarea
              v-model="livro.sinopse"
              placeholder="Sinopse"
              label="Sinopse"
              :rules="rule"
              required
              outlined
            />
          </v-col>
        </v-row>
          <v-row>
          <v-col>
            <v-autocomplete
            v-model="livro.idCategoria"
            :items="categorias"
            outlined
            label="ID Categoria"
            :rules="rule"
            required
            item-text="nome"
            item-value="id"
          ></v-autocomplete>
          </v-col>
          <v-col>
            <v-autocomplete
            v-model="livro.idAutor"
            :items="autores"
            outlined
            label="ID Autor"
            :rules="rule"
            required
            item-text="nome"
            item-value="id"
          ></v-autocomplete>
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
      to="/livros"
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
      livro: {
        id: null,
        titulo: null,
        sinopse: null,
        emprestado: null,
        idCategoria: null,
        idAutor: null,
      },
      autores: [],
      categorias: [],
      rule: [
        v => !!v || 'Esse campo é obrigatório'
      ]
    }
  },
    
  created () {
    if (this.$route?.params?.id) {
    this.getById(this.$route.params.id)
    }
    this.getAutores();
    this.getCategorias();
  },

  methods: {
    async persistir () {
      try {
         let livro = {
        titulo: this.livro.titulo,
        sinopse: this.livro.sinopse,
        emprestado: this.livro.emprestado,
        idCategoria: this.livro.idCategoria,
        idAutor: this.livro.idAutor
        };

        if(!this.livro.id){  
          let response = await this.$axios.$post('http://localhost:3333/livro', livro);
          this.$router.push('/livros')
          return this.$toast.success(`${response.titulo} cadastrado com sucesso`)
        }

        await this.$axios.$post(`http://localhost:3333/livro/${this.livro.id}`, livro )
        this.$router.push('/livros')
        this.$toast.success('Cadastro atualizado com sucesso!');
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!');
        
      }
    },
   
   async getAutores () {
      this.autores = await this.$axios.$get('http://localhost:3333/autor');
    },
    async getCategorias () {
      this.categorias = await this.$axios.$get('http://localhost:3333/categorias');
    },
    async getById (id) {
      this.livro= await this.$axios.$get(`http://localhost:3333/livro/${id}`);
    } 
  }
}
</script>
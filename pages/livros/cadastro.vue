<template>
  <v-container>
    <h1 style="text-align: center">Cadastro de Livros</h1>
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
              v-model="categoria.titulo"
              placeholder="Titulo"
              label="Titulo"
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-textarea
              v-model="categoria.sinopse"
              placeholder="Sinopse"
              label="Sinopse"
              outlined
            />
          </v-col>
        </v-row>
          <v-row>
          <v-col>
            <v-autocomplete
            v-model="categoria.idCategoria"
            :items="categorias"
            outlined
            label="ID Categoria"
            item-text="nome"
            item-value="id"
          ></v-autocomplete>
          </v-col>
          <v-col>
            <v-autocomplete
            v-model="categoria.idAutor"
            :items="autores"
            outlined
            label="ID Autor"
            item-text="nome"
            item-value="id"
          ></v-autocomplete>
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
      categoria: {
        id: null,
        titulo: null,
        sinopse: null,
        emprestado: null,
        idCategoria: null,
        idAutor: null,
      },
      autores: [],
      categorias: []
    }
  },
    
    created () {
    this.getAutores();
    this.getCategorias();
  },

  methods: {
    async cadastrar () {
      let categoria = {
        titulo: this.categoria.titulo,
        sinopse: this.categoria.sinopse,
        emprestado: this.categoria.emprestado,
        idCategoria: this.categoria.idCategoria,
        idAutor: this.categoria.idAutor
      };
      let response = await this.$axios.$post('http://localhost:3333/livro', categoria);
      console.log(response);
    },
   
   async getAutores () {
      this.autores = await this.$axios.$get('http://localhost:3333/autor');
    },
    async getCategorias () {
      this.categorias = await this.$axios.$get('http://localhost:3333/categorias');
    } 
  }
}
</script>
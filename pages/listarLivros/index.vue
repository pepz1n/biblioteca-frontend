<template>
  <v-container>
    <h1 style="text-align: center">Consultar Status Do Livro</h1>
    <hr>
    <v-form>
      <v-container>
        <v-row>
          <v-col>
            <v-autocomplete
            v-model="categoria.id"
            :items="livros"
            outlined
            label="ID Livro"
            required
            item-text="titulo"
            item-value="id"
          ></v-autocomplete>
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-btn
      outlined
      @click="verificar"
      color="green"
    >
      procurar
    </v-btn>
    <template v-if="emprestado">
      <v-card
        class="mx-auto"
        max-width="344"
      >
        <v-card-text>
          <div>ID: {{this.pesquisa[0].Emprestimo.id}}</div>
          <p class="text-h4 text--primary">
            {{this.pesquisa[0].Emprestimo.titulo}}
          </p>
          <p>Status do Livro:</p>
          <div class="text--primary">
            <strong style="color: red">EMPRESTADO</strong>
          </div>
        </v-card-text>
        <v-card-actions>
          <v-btn
            text
            color="teal accent-4"
            @click="reveal = true"
          >
            Veja detalhes do emprestimo
          </v-btn>
        </v-card-actions>

        <v-expand-transition>
          <v-card
            v-if="reveal"
            class="transition-fast-in-fast-out v-card--reveal"
            style="height: 100%;"
          >
            <v-card-text class="pb-0">
              <p class="text-h5 text--primary">
                Nome de quem está:
              </p>
              <p>{{this.nomeUsuario}} </p>
            </v-card-text>
            <v-card-text class="pb-0" style="margin-top: -7%">
              <p class="text-h5 text--primary">
                Data do empréstimo
              </p>
              <p>{{this.pesquisa[0].Emprestimo.created_at}} </p>
            </v-card-text>
            <v-card-actions class="pt-0">
              <v-btn
                text
                color="teal accent-4"
                @click="reveal = false"
              >
                Close
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-expand-transition>
      </v-card>
    </template>
    <template v-if="!emprestado">
      <v-card
        class="mx-auto"
        max-width="344"
      >
        <v-card-text>
          <div>ID: {{this.livros.id}}</div>
          <p class="text-h4 text--primary">
            {{this.livros.titulo}}
          </p>
          <p>Status do Livro:</p>
          <div class="text--primary">
            <strong style="color: green">DISPONÍVEL</strong>
          </div>
        </v-card-text>
        <v-card-actions>
          <v-btn
            text
            color="teal accent-4"
            @click="reveal = true"
          >
            Veja detalhes do emprestimo
          </v-btn>
        </v-card-actions>

        <v-expand-transition>
          <v-card
            v-if="reveal"
            class="transition-fast-in-fast-out v-card--reveal"
            style="height: 100%;"
          >
            <v-card-text class="pb-0">
              <p class="text-h5 text--primary">
                Nome de quem está:
              </p>
              <p>Talvez Você</p>
            </v-card-text>
            <v-card-text class="pb-0" style="margin-top: -7%">
              <p class="text-h5 text--primary">
                Data do empréstimo
              </p>
              <p>Hoje?</p>
            </v-card-text>
            <v-card-actions class="pt-0">
              <v-btn
                text
                color="teal accent-4"
                @click="reveal = false"
              >
                Close
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-expand-transition>
      </v-card>
    </template>
  </v-container>
</template>

<script>
export default {
  name: 'ListarLivrosPages',
  data(){
    return{
      nomeUsuario: null,
      reveal: false,
      emprestado: null,
      pesquisa: [],
      headers: [
        {
          text: 'Código',
          align: 'center',
          sortable: true,
          value: 'Emprestimo.id',
        },
        {
          text: 'Titulo',
          align: 'center',
          sortable: true,
          value: 'Emprestimo.titulo',
        },
        {
          text: 'Status',
          align: 'center',
          sortable: true,
          value: 'message',
        },
      ],
      categoria: {
        id: null
      },
      livros: []
    }
  },


  created (){
    this.getLivros()
  },


  methods:{

    async verificar (){
      try {
        let categoria ={
          idLivro: this.categoria.id
        };

        let socorro = await this.$axios.$post('http://localhost:3333/emprestimo/verificar', categoria)
        console.log(await this.$axios.$post('http://localhost:3333/emprestimo/verificar', categoria));
        this.emprestado = socorro[0].emprestado
        this.pesquisa = socorro
        let pessoa = await this.$axios.$post(`http://localhost:3333/usuario/${this.pesquisa[0].Emprestimo.id_usuario}`)
        this.nomeUsuario = pessoa.data.nome
        // console.log(socorro);
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!');
      }
    },

    async getLivros () {
      let forget = await this.$axios.$get('http://localhost:3333/livro');
       forget.forEach(element => {
        this.livros.push(element)
       }); 
    }  
  }
}
</script>

<style>
.v-card--reveal {
  bottom: 0;
  opacity: 1 !important;
  position: absolute;
  width: 100%;
}

</style>




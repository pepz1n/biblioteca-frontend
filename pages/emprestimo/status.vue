<template>
  <v-container>
    <h1 style="text-align: center">Cadastro de Categorias</h1>
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
          <v-col>
            <v-text-field
              v-model="nulo"
              placeholder="Devolução"
              label="Devolução"
              :rules="rule"
              disabled
              required
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <template>
              <div>
                <v-menu
                  ref="menu"
                  v-model="menu"
                  :close-on-content-click="false"
                  transition="scale-transition"
                  offset-y
                  min-width="auto"
                  disabled
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-text-field
                      v-model="categoria.prazo"
                      label="Prazo"
                      readonly
                      v-bind="attrs"
                      v-on="on"
                      outlined
                      disabled
                    ></v-text-field>
                  </template>
                  <v-date-picker
                    v-model="date"
                    :active-picker.sync="activePicker"
                    :max="(new Date(Date.now() + (new Date()).getTimezoneOffset() * 600000000)).toISOString().substr(0, 10)"
                    min="2022-01-01"
                    @change="save"
                  ></v-date-picker>
                </v-menu>
              </div>
            </template>
          </v-col>
          <v-col>
            <v-autocomplete
              chips
              v-model="categoria.livros"
              :items="categoria.livros"
              clearable
              deletable-chips
              multiple
              outlined
              label="ID Livro"
              item-text="titulo"
              item-value="id"
              disabled
            ></v-autocomplete>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-autocomplete
            v-model="categoria.idUsuario"
            :items="usuarios"
            outlined
            label="ID Usuarios"
            :rules="rule"
            required
            item-text="nome"
            item-value="id"
            disabled
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
      to="/emprestimo"
      color="red"
    >
      cancelar
    </v-btn>
  </v-container>
</template>

<script>
export default {
  name: 'StatusEmprestimoPage',

  data () {
    return {
      valid: false,
      nulo: "null",
      activePicker: null,
      menu: false,
      categoria: {
        id: null,
        idUsuario: null,
        nome: null,
        prazo: null,
        livros: []
      },
      categorias: {
        id: null
      },
      usuarios: [],
      rule: [
        v => !!v || 'Esse campo é obrigatório'
      ]
    }
  },
  watch: {
    menu (val) {
      val && setTimeout(() => (this.activePicker = 'YEAR'))
    },
  },
  created () {
    this.getLivros()
    this.getUsuarios();
    if (this.$route?.params?.id) {
      this.getById(this.$route.params.id)
    }
  },

  methods: {
    async persistir() {
      try {
         if (!this.valid) {
          return this.$toast.warning('O formulário de cadastro não é válido!')
        }

        let categoria = {
          prazo: this.prazo,
          devolucao: null,
          idUsuario: this.categoria.idUsuario,
          livros: this.categorias.id
        };
        if(!this.categoria.id){
          let response = await this.$axios.$post('http://localhost:3333/emprestimo', categoria);
          this.$router.push('/emprestimo')
          return this.$toast.success(`${response.id} cadastrado com sucesso`)
        }


        await this.$axios.$post(`http://localhost:3333/emprestimo/${this.categoria.id}`, categoria )
        this.$router.push('/emprestimo')
        this.$toast.success('Cadastro atualizado com sucesso!');
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!');
        
      }

    },
    save (date) {
      this.$refs.menu.save(date)
    },
   
    async getById (id) {
      this.categoria = await this.$axios.$get(`http://localhost:3333/emprestimo/${id}`);
      console.log(await this.$axios.$get(`http://localhost:3333/emprestimo/${id}`));
    },
    
    async getLivros () {
      let forget = await this.$axios.$get('http://localhost:3333/livro');
       forget.forEach(element => {
        this.livros.push(element)
       }); 
    },
    async getUsuarios () {
      let forget = await this.$axios.$get('http://localhost:3333/usuario');
       forget.forEach(element => {
        this.usuarios.push(element)
       }); 
    } 
  }
}
</script>
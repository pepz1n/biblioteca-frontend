<template>
  <v-container>
    <h1 style="text-align: center">Status de emprestimo</h1>
    <hr>
    <v-form v-model="valid">
      <v-container>
        <v-row>
          <v-col>
            <v-text-field
              v-model="emprestimo.id"
              placeholder="codigo"
              label="codigo"
              disabled
              outlined
            />
          </v-col>
          <v-col>
            <v-text-field
              v-model="emprestimo.devolucao"
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
                      v-model="emprestimo.prazo"
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
              v-model="emprestimo.livros"
              :items="livros"
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
            v-model="emprestimo.idUsuario"
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
      Voltar
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
      emprestimo: {
        id: null,
        devolucao: null,
        idUsuario: null,
        nome: null,
        prazo: null,
        livros: []
      },
      emprestimos: {
        id: null
      },
      usuarios: [],
      livros: [],
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

        let emprestimo = {
          prazo: this.prazo,
          devolucao: this.emprestimo.devolucao,
          idUsuario: this.emprestimo.idUsuario,
          livros: this.emprestimos.id
        };
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!');
        
      }

    },
    save (date) {
      this.$refs.menu.save(date)
    },
   
    async getById (id) {
      this.emprestimo = await this.$axios.$get(`http://localhost:3333/emprestimo/${id}`);
      if(!this.emprestimo.devolucao){
        this.emprestimo.devolucao = "Não Devolvido"
      }
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
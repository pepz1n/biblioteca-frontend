<template>
  <v-container>
    <h1 style="text-align: center">Cadastro de emprestimos</h1>
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
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-text-field
                      v-model="emprestimo.prazo"
                      label="Prazo"
                      prepend-icon="mdi-calendar"
                      readonly
                      v-bind="attrs"
                      v-on="on"
                      :disabled = "disabilitar"
                      outlined
                    ></v-text-field>
                  </template>
                  <v-date-picker
                    v-model="emprestimo.prazo"
                    :active-picker.sync="activePicker"
                    :max="(new Date(Date.now() + (new Date()).getTimezoneOffset() * 600000000)).toISOString().substr(0, 10)"
                    :min="new Date(Date.now()).toISOString().substr(0, 10)"
                    @change="save"
                    rules="rule"
                    :disabled = "disabilitar"
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
              :disabled = "disabilitar"
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
            :disabled = "disabilitar"
          ></v-autocomplete>
          </v-col>
        </v-row>
        
      </v-container>
 </v-form>
   <v-btn
      outlined
      @click="persistir"
      color="green"
      v-if="disabilitar == null"
    >
      cadastrar
    </v-btn>
    <v-btn
      outlined
      to="/emprestimo"
      color="red"
      v-if="disabilitar == null"
    >
      cancelar
    </v-btn>
    <v-btn
      outlined
      to="/emprestimo"
      color="red"
      v-if="disabilitar"
    >
      voltar
    </v-btn>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroemprestimosPage',

  data () {
    return {
      disabilitar: null,
      nulo: "null",
      activePicker: null,
      menu: false,
      message: null,
      emprestimo: {
        id: null,
        idUsuario: null,
        nome: null,
        prazo: null,
        livros: [],
        devolucao: null
      },
      emprestimos: {
        id: null
      },
      livros: [],
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
    this.getUsuarios();
    if (this.$route?.params?.id) {
      this.getById(this.$route.params.id)
      this.getLivros()
      this.disabilitar = true
      return
    }
    this.getLivrosDisponiveis()
  },

  methods: {
    async persistir() {
      let response = null
      try {
        let emprestimo = {
          prazo: this.emprestimo.prazo,
          devolucao: null,
          idUsuario: this.emprestimo.idUsuario,
          livros: this.emprestimo.livros
        };
        if(!this.emprestimo.id){
          response = await this.$axios.$post('http://localhost:3333/emprestimo', emprestimo);
          console.log(response);
          if(response.type == 'sucess'){
            this.$router.push('/emprestimo')
            return this.$toast.success(`${response.dados.id} cadastrado com sucesso`)
          }else{
            return this.$toast.error(`${response.message}`)
          }
        }
      } catch (error) {
        return error
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
      let livro = await this.$axios.$get('http://localhost:3333/livro');
       livro.forEach(element => {
        this.livros.push(element)
       }); 
    },
    async getLivrosDisponiveis () {
      let livro = await this.$axios.$get('http://localhost:3333/livros-disponiveis');
      console.log(livro);
       livro.forEach(element => {
        this.livros.push(element)
       }); 
    },
    async getUsuarios () {
      let usuario = await this.$axios.$get('http://localhost:3333/usuario');
       usuario.forEach(element => {
        this.usuarios.push(element)
       }); 
    } 
  }
}
</script>
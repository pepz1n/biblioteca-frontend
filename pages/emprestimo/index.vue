<template>
  <v-container>
    <h1>Consulta de emprestimos</h1>
    <hr>
    <v-row style="margin-top: 2%">
      <v-col>
        <v-btn
          color="blue"
          outlined
          @click="getEmprestimos"
        >
          Pesquisar
          </v-btn>
      </v-col>
     <v-col
        cols="2"
      >
        <v-btn
          to="/emprestimo/cadastro"
          color="green"
        >
         <v-icon>
          mdi-plus
         </v-icon> 
         cadastro
        </v-btn>
      </v-col>
    </v-row>
    <v-container>
      <v-row>
      <v-col
      cols="max"
      >
      <v-data-table
        :headers="headers"
        :items="emprestimos"
        :items-per-page="10"
        class="elevation-1"
      >
        <template v-slot:item.devolucao="{ item }">
          <p 
            :style="item.devolucao === 'Devolvido' ? 'color: green' : 'color: red'"
          > {{ item.devolucao }} </p>
        </template>
        <template v-slot:item.actions="{ item }">
          <v-icon
            small
            class="mr-2"
            @click="editar(item)"
          >
            mdi-magnify
          </v-icon>
          <v-icon
            small
            @click="deletar(item)"
          >
            mdi-delete
          </v-icon>
          <v-icon
            small
            @click="terminarEmprestimo(item)"
          >
            mdi-check
          </v-icon>
        </template>
      </v-data-table>
      </v-col>
    </v-row>
    </v-container>
  </v-container>
</template>

<script>
export default {
  name: 'ConsultaEmprestimoPage',

  data () {
    return{
      headers: [
        {
          text: 'Código',
          align: 'center',
          sortable: true,
          value: 'id',
        },
        {
          text: 'ID usuario',
          align: 'center',
          sortable: true,
          value: 'idUsuario',
        },
        {
          text: 'Prazo',
          align: 'center',
          sortable: true,
          value: 'prazo',
        },
        {
          text: 'Devolucao',
          align: 'center',
          sortable: true,
          value: 'devolucao',
        },
        { text: "", value: "actions" }
      ],
      emprestimos: []
    }
  },

  methods: {
    getEmprestimos: async function () {
      this.emprestimos = await this.$axios.$get(`http://localhost:3333/emprestimo`)
      this.emprestimos.forEach(emprestimo => {
        if(emprestimo.devolucao){
          emprestimo.devolucao = "Devolvido"
        }else{
          emprestimo.devolucao = "Não Devolvido"
        }
      });
    },
     async deletar (autor) {
      try {
        if (confirm(`Deseja deletar o emprestimo id: ${autor.id} - ${autor.prazo}?`)) {
          let response = await this.$axios.$post('http://localhost:3333/emprestimo/deletar', { id: autor.id });
          this.$toast.success(response.message)
          this.getEmprestimos();
        }
      } catch (error) {
        this.$toast.error(error.message)
      }
    },
    async editar (emprestimoEdi) {
      this.$router.push({
        name: 'emprestimo-cadastro',
        params: { id: emprestimoEdi.id }
      });
    },
    async terminarEmprestimo(emprestimoTerminar){
      let id = emprestimoTerminar.id;
      let emprestimoTerminarJSON = {
        devolucao: new Date(Date.now()).toISOString().substring(0,10)
      }
      try {
        if(confirm(`Deseja Terminar o emprestimo de id: ${id} ?`)){
          await this.$axios.$post(`http://localhost:3333/emprestimo/${id}`, emprestimoTerminarJSON )
          this.$router.push('/emprestimo')
          this.$toast.success(`Emprestimo finalizado com a data de ${new Date(Date.now()).toISOString().substring(0,10)}`);
          this.getEmprestimos()
        }
      } catch (error) {
        this.$toast.error(error.message)
      } 

    }

  },
  beforeMount(){
    this.getEmprestimos()
  }
  

}
</script>

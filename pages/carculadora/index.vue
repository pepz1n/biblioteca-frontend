<template>
  <v-container>
    <v-container style="padding: 20%">
      <v-row>
        <v-col
          cols="8"
        >
            <h4>{{digitos.join(' ')}}</h4>
        </v-col>
        <v-col>
          <v-btn
            @click="reload"
          >
            limpar
          </v-btn>
        </v-col>
      </v-row>
      <v-row>
        <v-col
        cols="10"
        >
          <v-text-field
          v-model="visor"
          disabled
          outlined
          placeholder="0"
          >
       </v-text-field>
       </v-col>
      </v-row>
      <v-row>
        <v-col>
          <v-btn
             color="dark"
             tile
             @click="mostrarNumero(7)"
          >7</v-btn>
        </v-col>
        <v-col class="botoes">
          <v-btn
             color="dark"
             tile
             @click="mostrarNumero(8)"
          >8</v-btn>
        </v-col>
        <v-col class="botoes">
          <v-btn
             color="dark"
             tile
             @click="mostrarNumero(9)"
          >9</v-btn>
        </v-col>
        <v-col class="botoes">
          <v-btn
             color="red"
             tile
             @click="soma"
          >+</v-btn>
        </v-col>
      </v-row>
      <v-row>
        <v-col>
          <v-btn
             color="dark"
             tile
             @click="mostrarNumero(4)"
          >4</v-btn>
        </v-col>
        <v-col class="botoes">
          <v-btn
             color="dark"
             tile
             @click="mostrarNumero(5)"
          >5</v-btn>
        </v-col>
        <v-col class="botoes">
          <v-btn
             color="dark"
             tile
             @click="mostrarNumero(6)"
          >6</v-btn>
        </v-col>
        <v-col class="botoes">
          <v-btn
             color="red"
             tile
             @click="subtracao"
          >-</v-btn>
        </v-col>
      </v-row>
      <v-row>
        <v-col>
          <v-btn
             color="dark"
             tile
             @click="mostrarNumero(1)"
          >1</v-btn>
        </v-col>
        <v-col class="botoes">
          <v-btn
             color="dark"
             tile
             @click="mostrarNumero(2)"
          >2</v-btn>
        </v-col>
        <v-col class="botoes">
          <v-btn
             color="dark"
             tile
             @click="mostrarNumero(3)"
          >3</v-btn>
        </v-col>
        <v-col class="botoes">
          <v-btn
             color="red"
             tile
             @click="divisao"
          >/</v-btn>
        </v-col>
      </v-row>
      <v-row>
        <v-col
        cols="5 "
        >
          <v-btn
             color="dark"
             tile
             block
             @click="mostrarNumero(0)"
          >0</v-btn>
        </v-col>
        <v-col>
          <v-btn
             color="green"
             tile
             @click="igual"
             class="forget"
          >=</v-btn>
        </v-col>
        <v-col class="botoes">
          <v-btn
             color="red"
             tile
             @click="multiplicacao"
          >*</v-btn>
        </v-col>
      </v-row>
    </v-container>
  </v-container>
</template>

<script>
export default {
  name: 'CarculadoraPage',

  data(){
    return{
      visor: null,
      resposta: 0,
      numero: [],
      digitos:[],
    }
  },






  methods:{
    reload() {
      window.location.reload()
    },
    mostrarNumero (numeroDigitado){
    
      this.numero.push(numeroDigitado)
      this.visor = this.numero.join('')
      this.aux = this.numero.join('')
      console.log(this.digitos);
    },

    soma (){
      this.digitos.push(Number(this.aux))
      this.aux = null
      this.visor = null
      this.numero = []
      this.digitos.push('+')
    },
    subtracao (){
      this.digitos.push(Number(this.aux))
      this.aux = null
      this.visor = null
      this.numero = []
      this.digitos.push('-')
    },
    divisao (){
      this.digitos.push(Number(this.aux))
      this.aux = null
      this.visor = null
      this.numero = []
      this.digitos.push('/')
    },
    multiplicacao(){
      this.digitos.push(Number(this.aux))
      this.aux = null
      this.visor = null
      this.numero = []
      this.digitos.push('*')
    },
   
    igual(){
      this.digitos.push(Number(this.aux))
      this.aux = null
      this.visor = null
      this.numero = []
      this.digitos.push('=')

      for (let i = 0; i < this.digitos.length; i++) {
        if(i == 0){
          this.resposta += this.digitos[0]
        }
        if(this.digitos[i] == '/'  && this.digitos[i+1] == 0){
          alert("Se ta de sacanagem dividindo por 0")
          this.reload()
          return
        }
        if(i%2!=0){
            switch (this.digitos[i]) {
              case '+':
                  this.resposta += this.digitos[i+1]
                break;
              case '-':
                this.resposta -= this.digitos[i+1]
                break;
              case '*':
                this.resposta *= this.digitos[i+1];
                break
              case '/':
                this.resposta = this.resposta/this.digitos[i+1]
              default:
                break;
            }
        }
      }
      this.visor = this.resposta
    }
  }
}
</script>


<style>
  .botoes{
    margin-left:  -17% ;
    };
  .forget{
    color: green;
  }

</style>

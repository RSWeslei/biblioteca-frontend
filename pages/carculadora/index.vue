<template>
  <v-container>
    <v-card class="card">
      <v-container class="screen">
        <v-text-field
          solo
          disabled
          height="50px"
          v-model="contaDisplay"
        >

        </v-text-field>
      </v-container>
      <v-container>
        <v-card>
          <v-col class="numberPad">
            <v-col align="center">
              <v-btn
                @click="adicionarConta('9')"
              >9</v-btn>
              <v-btn
                @click="adicionarConta('8')"
              >8</v-btn>
              <v-btn
                @click="adicionarConta('7')"
              >7</v-btn>
               <v-btn
                @click="adicionarConta('/')"
              >%</v-btn>
            </v-col>
            <v-col align="center">
              <v-btn
                @click="adicionarConta('4')"
              >4</v-btn>
              <v-btn
                @click="adicionarConta('5')"
              >5</v-btn>
              <v-btn
                @click="adicionarConta('6')"
              >6</v-btn>
               <v-btn
                @click="adicionarConta('*')"
              >x</v-btn>
            </v-col>
            <v-col align="center">
              <v-btn
                @click="adicionarConta('1')"
              >1</v-btn>
              <v-btn
                @click="adicionarConta('2')"
              >2</v-btn>
              <v-btn
                @click="adicionarConta('3')"
              >3</v-btn>
               <v-btn
                @click="adicionarConta('-')"
              >-</v-btn>
            </v-col>
            <v-col align="center">
              <v-btn
                @click="adicionarConta('0')"
              >0</v-btn>
              <v-btn
                @click="resetar"
                color="red"
              >CE</v-btn>
              <v-btn
                @click="calcular"
                color="green"
              >=</v-btn>
               <v-btn
                @click="adicionarConta('+')"
              >+</v-btn>
            </v-col>
          </v-col>
        </v-card>
      </v-container>
    </v-card>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    conta: [],
    contaDisplay: [],
    operacoes: ['*', '/', '-', '+'],
    calculado: false,
    operadores: [],
    numeros: []
  }),
  methods: {
    adicionarConta (value)
    {
      // if (this.calculado){
      //   this.calculado = false
      //   this.contaDisplay = []
      //   this.conta = []
      // }
      if (this.conta.length == 0 && this.operacoes.includes(value)){
        console.log(this.conta);
        return
      }
      let last = this.conta[this.conta.length-1]
      let lastNum = this.numeros[this.numeros.length-1]
      if (this.operacoes.includes(value)){
        if (this.operacoes.includes(last)){
          this.conta.pop()
          this.operadores.pop()
          console.log(this.numeros.push(value))
          console.log('iouawd');
        }
        this.operadores.push(value)
      }
      else {

        this.numeros[this.numeros.length-1] += value
      }
     
      console.log(last);


      this.conta.push(value)
      this.contaDisplay = this.conta.join('')
      console.log(this.operadores);
      console.log(this.numeros);
    },
    calcular () {
      try {
        if (this.operacoes.includes(this.conta[0])){
          this.conta.shift()
        }
        if (this.operacoes.includes(this.conta[this.conta.length-1])){
          this.conta.pop()
        }
        let conta = this.conta.join('')
        let operadores = conta.split(/\d+/g)
        let numeros = conta.match(/\d+/g)
        
  
        operadores.forEach((operador, index) => {
          if (operador == ''){
            operadores.splice(index, 1)
          }
        })
        console.log(this.conta);
  
        let resultado = (operadores.length == 0) ? 0 : this.conta
  
        console.log(numeros);
        console.log(operadores);
  
        for (let i = 0; i < operadores.length; i++) {
          switch (operadores[i]) {
            case '+':
                resultado = Number(numeros[0]) + Number(numeros[1])
              break;
            case '-':
                resultado = Number(numeros[0]) - Number(numeros[1])
              break
            case '*':
                resultado = Number(numeros[0]) * Number(numeros[1])
              break
            case '/':
                resultado = Number(numeros[0]) / Number(numeros[1])
              break
  
          }
          numeros.splice(0, 2)
          numeros.unshift(resultado)
        }
        this.calculado = true
        this.conta = [resultado]
        this.contaDisplay = [resultado]
      } catch (error) {
        console.log('Deu erro');
      }
    },
    resetar () {
      this.conta = []
      this.contaDisplay = []
    }
  }

}
</script>

<style>
  .card {
    width: 400px;
    height: 500px;
  }
  .screen {
    background-color: blue;
  }
  .numberPad {
    
  }
  .numberPadCol {
    padding-right: 100px;
  }
</style>
<template>
  <v-container>
    <v-card>
      <h1>Consulta de livro</h1>
      <hr>
      <v-form>
        <v-container>
          <v-autocomplete
            v-model="emprestimo.idLivro"
            outlined
            placeholder="Livros"
            label="Livros"
            rounded
            color="green"
            :items="livros"
            item-text="id"
            item-value="id"   
          ></v-autocomplete>
        </v-container>
        <v-container>
          <v-btn
            @click="consultarEmprestimo"
            color="green"
          >
            Consultar
          </v-btn>
        </v-container>
      </v-form>
      <v-container v-if="!emprestado">
        <v-row>
          <v-col>
            <h1
              style="color: green;"
            >
              {{ `O livro está dísponivel`}}
              <v-icon
                color="green"
              >
                mdi-thumb-up 
              </v-icon>
            </h1>
          </v-col>
        </v-row>
      </v-container>
      <v-container v-if="emprestado">
        <v-data-table
          :headers="headers"
          :items="emprestimo.emprestimos"
          :items-per-page="10"
          class="elevation-1"
        >
        </v-data-table>
      </v-container>
    </v-card>
  </v-container>
</template>

<script>

export default {
  data () {
    return {
      headers: [
        {
          text: 'Código',
          align: 'center',
          sortable: false,
          value: 'id'
        },
        {
          text: 'Prazo',
          align: 'center',
          sortable: false,
          value: 'prazo'
        },
        {
          text: 'Devolucão',
          align: 'center',
          sortable: false,
          value: 'devoluca'
        }
      ],
      emprestimo: {
        idLivro: null,
        emprestimos: []
      },
      livros: [],
      emprestado: null
    }
  },
  created () {
    this.getLivros()
  },
  methods: {
    async consultarEmprestimo () {
      if (this.emprestimo.idLivro == null){
        this.$toast.warning('Digite o nome do livro')
        return
      }
      let response = await this.$axios.$post('http://localhost:3333/emprestimos/existente', this.emprestimo)
      this.emprestimo = response
      this.emprestado = response.emprestado
    },
    async getLivros() {
      this.livros = await this.$axios.$get('http://localhost:3333/livros/')
    }
  }
}
</script>

<style>

</style>
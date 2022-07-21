<template>
  <v-container>
    <h1>Cadastro de Categorias</h1>
    <hr>
    <v-form>
      <v-container>
        <v-row>
          <v-col>
            <v-text-field
              v-model="categoria.id"
              rounded
              placeholder="Código"
              label="Código"
              outlined
              disabled
            ></v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="categoria.nome"
              rounded
              placeholder="Nome"
              color="green"
              label="Nome"
              outlined
            ></v-text-field>
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-btn
      @click="cadastrar"
      color="green"
    >
      Cadastrar
    </v-btn>
    <v-dialog
      style="padding-top: 1000"
      v-model="cadastrado"
      hide-overlay
      width="300"
      >
      <v-card
        :color="cadastroErro ? 'red' : 'green'"
      >
        <v-card-text
          
        >
          {{ cadastroErro ? 'Erro ao cadastrar a categoria' : 'Categoria cadastrado com sucesso' }}
        </v-card-text>
      </v-card>
    </v-dialog>
    <v-btn
      outlined
      to="/categorias"
    >
      Cancelar
    </v-btn>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroCategoriasPage',

  data () {
    return {
      categoria: {
        id: null,
        nome: null
      },
      cadastrado: false,
      cadastroErro: false
    }
  },
  
  methods: {
    async cadastrar () {
      try {
        let categoria = {
          nome: this.categoria.nome
        }
        let response = await this.$axios.$post('http://localhost:3333/categorias', categoria);
        this.cadastrado = true
      } catch (error) {
        this.cadastrado = true
        this.cadastroErro = true
      }
    }
  },

  watch: {
    cadastrado (val) {
      if (!val) return
      setTimeout(() => (this.cadastrado = false, this.cadastroErro = false), 3000)
    },
  }
}
</script>

<style>
  .v-dialog {
    margin-top: 800px;
    margin-right: 800px;
  }
</style>
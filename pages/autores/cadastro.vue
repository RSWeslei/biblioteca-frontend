<template>
  <v-container>
    <h1>Cadastro de Autores</h1>
    <hr>
    <v-form>
      <v-container>
        <v-row>
          <v-col>
            <v-text-field
              v-model="autor.id"
              rounded
              placeholder="Nome"
              color="green"
              label="Código"
              clearable
              outlined
              disabled
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="autor.nome"
              label="Nome"
              rounded
              placeholder="Nome"
              clearable
              outlined
              color="green"
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="autor.email"
              rounded
              placeholder="Email"
              label="Email"
              clearable
              outlined
              color="green"
            >
            </v-text-field>
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-btn
      color="green"
      @click="cadastrar"
    >
      Cadastrar
    </v-btn>
    <v-btn
      outlined
      to="/autores"
    >
      Cancelar
    </v-btn>
    <v-dialog
      v-model="cadastrado"
      hide-overlay
      width="300"
      >
      <v-card
        :color="cadastroErro ? 'red' : 'green'"
      >
        <v-card-text
          style="text-align: center"
        >
          {{ cadastroErro ? 'Erro ao cadastrar o autor' : 'Autor cadastrado com sucesso' }}
        </v-card-text>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroAutoresPage',

  data () {
    return {
      autor: {
        id: null,
        nome: null,
        email: null
      },
      cadastrado: false,
      cadastroErro: false
    }
  },
  
  methods: {
    async cadastrar () {
      try {
        let autor = {
          nome: this.autor.nome,
          email: this.autor.email
        }
        let response = await this.$axios.$post('http://localhost:3333/autores', autor);
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
      setTimeout(() => (this.cadastrado = false, this.cadastroErro = false), 4000)
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
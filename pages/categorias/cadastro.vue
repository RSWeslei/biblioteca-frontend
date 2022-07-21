<template>
  <v-container>
    <h1>Cadastro de Categorias</h1>
    <hr>
    <v-form v-model="valid">
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
              :rules="rule"
              required
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
      valid: false,
      categoria: {
        id: null,
        nome: null
      },
      rule: [
        v => !!v || 'Esse campo é obrigatório'
      ]
    }
  },
  
  methods: {
    async cadastrar () {
      try {
        if (!this.valid){
          return this.$toast.warning('O formulário de cadastro não é válido!')
        }
        let categoria = {
          nome: this.categoria.nome
        }
        await this.$axios.$post('http://localhost:3333/categorias', categoria);
        this.$toast.success('Categoria cadastrada com sucesso')
        this.$router.push('/categorias')
      } catch (error) {
        this.$toast.error('Erro ao cadastrar a categoria')
      }
    }
  }
}
</script>

<style>
  .v-dialog {
    margin-top: 800px;
    margin-right: 800px;
  }
</style>
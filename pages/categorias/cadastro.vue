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
      @click="persistir"
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

  created () {
    if (this.$route?.params?.id){
      this.getById(this.$route?.params?.id)
    }
  },
  
  methods: {
    async persistir () {
      try {
        if (!this.valid){
          return this.$toast.warning('O formulário de cadastro não é válido!')
        }
        let categoria = {
          nome: this.categoria.nome
        }

        if (!this.categoria.id){
          await this.$axios.$post('http://localhost:3333/categorias', categoria)
          this.$toast.success('Cadastro realizado com sucesso!')
          return this.$router.push('/categorias');
        }
        await this.$axios.$post(`http://localhost:3333/categorias/${this.categoria.id}`, categoria);
        this.$toast.success('Cadastro atualizado com sucesso!');
        return this.$router.push('/categorias');
      } catch (error) {
        this.$toast.error('Erro ao cadastrar a categoria')
      }
    },
    async getById (id) {
      this.categoria = await this.$axios.$get(`http://localhost:3333/categorias/${id}`);
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
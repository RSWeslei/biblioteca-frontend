<template>
  <v-container>
    <h1>Cadastro de Autores</h1>
    <hr>
    <v-form v-model="valid">
      <v-container>
        <v-row>
          <v-col>
            <v-text-field
              v-model="autor.id"
              rounded
              placeholder="Código"
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
              :rules="rule"
              required
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
              :rules="rule"
              required
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
  </v-container>
</template>

<script>
export default {
  name: 'CadastroAutoresPage',

  data () {
    return {
      valid: false,
      autor: {
        id: null,
        nome: null,
        email: null
      },
      rule: [
        v => !!v || 'Este campo é obrigatório!!'
      ]
    }
  },
  created () {
    if (this.$route?.params?.id){
      this.getById(this.$route?.params?.id)
    }
  },
  
  methods: {
    async cadastrar () {
      try {
        if (!this.valid){
          return this.$toast.warning('O formulário de cadastro não é válido!')
        }
        let autor = {
          nome: this.autor.nome,
          email: this.autor.email
        }
        if (!this.autor.id){
          await this.$axios.$post('http://localhost:3333/autores', autor);
          this.$toast.success('Autor cadastrado com sucesso')
          return this.$router.push('/autores')
        }

        await this.$axios.$post(`http://localhost:3333/autores/${this.autor.id}`, autor);
        this.$toast.success('Cadastro atualizado com sucesso!');
        return this.$router.push('/autores');
      } catch (error) {
        this.$toast.error('Erro ao cadastrar o autor')
      }
    },
    async getById (id) {
      this.autor = await this.$axios.$get(`http://localhost:3333/autores/${id}`);
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
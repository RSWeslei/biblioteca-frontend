<template>
  <v-container>
    <h1>Cadastro de Empréstimos</h1>
    <hr>
    <v-form v-model="valid">
      <v-container>
        <v-row>
          <v-col>
            <v-text-field
              rounded
              placeholder="Código"
              label="Código"
              outlined
              disabled
            ></v-text-field>
          </v-col>
        </v-row>
        <v-col>
          Prazo
        </v-col>
        <v-row>
          <v-col>
            <v-autocomplete
              v-model="emprestimo.idUsuario"
              rounded
              placeholder="Usuario"
              outlined
              color="green"
              :rules="rule"
              locale
              title="Usuario"
              required
              item-value="id"
              item-text="nome"
              :items="usuarios"
            ></v-autocomplete>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-date-picker
              v-model="emprestimo.prazo"
              color="green"
              :rules="rule"
              locale
              title="Prazo"
              required
            ></v-date-picker>
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
      to="/emprestimos"
    >
      Cancelar
    </v-btn>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroEmprestimoPage',

  data () {
    return {
      valid: false,
      emprestimo: {
        prazo: null,
        devolucao: null,
        idUsuario: null,
        livros: []
      },
      usuarios: [],
      rule: [
        v => !!v || 'Esse campo é obrigatório'
      ]
    }
  },

  created () {
    if (this.$route?.params?.id){
      this.getById(this.$route?.params?.id)
    }
    this.getUsuarios()
  },
  
  methods: {
    async cadastrar () {
      try {
        if (!this.valid){
          return this.$toast.warning('O formulário de cadastro não é válido!')
        }
        await this.$axios.$post(`http://localhost:3333/emprestimos/`, emprestimo)
        
      } catch (error) {
        this.$toast.error('Erro ao cadastrar a emprestimo')
      }
    },
    async getById (id) {
      this.emprestimo = await this.$axios.$get(`http://localhost:3333/emprestimos/${id}`);
    },
    async getUsuarios(){
      this.usuarios = await this.$axios.$get(`http://localhost:3333/usuarios`);
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
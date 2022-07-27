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
          <v-col>
            Prazo
          </v-col>
        <v-row>
          <v-col>
            <v-date-picker
              v-model="emprestimo.prazo"
              color="green"
              :rules="rule"
              title="Prazo"
              required
            ></v-date-picker>
          </v-col>
          <v-col>
            <v-autocomplete
              v-model="emprestimo.livros"
              rounded
              multiple
              placeholder="Livros"
              outlined
              color="green"
              :rules="rule"
              title="Livros"
              required
              item-value="id"
              item-text="titulo"
              :items="livros"
            ></v-autocomplete>
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
      livros: [],
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
    this.getLivros()
  },
  
  methods: {
    async cadastrar () {
      try {
        if (!this.valid){
          return this.$toast.warning('O formulário de cadastro não é válido!')
        }
        // if (emprestimo.prazo <= new Date(Date.now())){
        //   this.$toast.warning('A data não pode ser menor do que hoje')
        //   return
        // }

        await this.$axios.$post(`http://localhost:3333/emprestimos/`, this.emprestimo)
        this.$toast.success('Emprestimo cadastrado com sucesso!');
        return this.$router.push('/emprestimos');
      } catch (error) {
        this.$toast.error('Erro ao cadastrar a emprestimo')
      }
    },
    async getById (id) {
      this.emprestimo = await this.$axios.$get(`http://localhost:3333/emprestimos/${id}`);
    },
    async getUsuarios(){
      this.usuarios = await this.$axios.$get(`http://localhost:3333/usuarios`);
    },
    async getLivros(){
      this.livros = await this.$axios.$get(`http://localhost:3333/livros/avaliable`);
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
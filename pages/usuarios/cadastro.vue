<template>
  <v-container>
    <h1>Cadastro de Usuarios</h1>
    <hr>
    <v-form v-model="valid">
      <v-container>
        <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.id"
              rounded
              placeholder="Nome"
              color="green"
              outlined
              disabled
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.nome"
              rounded
              placeholder="Nome"
              color="green"
              label="Nome"
              clearable
              outlined
              :rules="rule"
              required
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.cpfcnpj"
              rounded
              placeholder="CPF/CNPJ"
              color="green"
              label="CPF/CNPJ"
              clearable
              outlined
              :rules="rule"
              required
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.email"
              rounded
              placeholder="Email"
              color="green"
              label="Email"
              clearable
              outlined
              :rules="rule"
              required
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.telefone"
              rounded
              placeholder="Telefone"
              color="green"
              label="Telefone"
              clearable
              outlined
              :rules="rule"
              required
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
      to="/usuarios"
    >
      Cancelar
    </v-btn>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroUsuariosPage',

  data () {
    return {
      valid: false,
      usuario: {
        id: null,
        nome: null,
        cpfcnpj: null,
        email: null,
        telefone: null
      },
      rule: [
        v => !!v || 'Este campo é obrigatório!!'
      ]
    }
  },
  
  methods: {
    async cadastrar () {
      try {
        if (!this.valid){
          return this.$toast.warning('O formulário de cadastro não é válido!')
        }
        let usuario = {
          nome: this.usuario.nome,
          cpfcnpj: this.usuario.cpfcnpj,
          email: this.usuario.email,
          telefone: this.usuario.telefone,
        }
        await this.$axios.$post('http://localhost:3333/usuarios', usuario);
        this.$toast.success('Usuário cadastrado com sucesso')
        this.$router.push('/usuarios')
      } catch (error) {
        this.$toast.error('Erro ao cadastrar o usuário')
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
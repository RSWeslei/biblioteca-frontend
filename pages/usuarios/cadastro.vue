<template>
  <v-container>
    <h1>Cadastro de Usuarios</h1>
    <hr>
    <v-form>
      <v-container>
        <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.id"
              solo
              label="Código"
              clearable
              outlined
              disabled
              color="white"
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.nome"
              solo
              label="Nome"
              clearable
              outlined
              color="white"
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.cpfcnpj"
              solo
              label="CPF / CNPJ"
              clearable
              outlined
              color="white"
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.email"
              solo
              label="Email"
              clearable
              outlined
              color="white"
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.telefone"
              solo
              label="Telefone"
              clearable
              outlined
              color="white"
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
          Usuario cadastrado
        </v-card-text>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroUsuariosPage',

  data () {
    return {
      usuario: {
        id: null,
        nome: null,
        cpfcnpj: null,
        email: null,
        telefone: null
      },
      cadastrado: false,
      cadastroErro: false
    }
  },
  
  methods: {
    async cadastrar () {
      try {
        let usuario = {
          nome: this.usuario.nome,
          cpfcnpj: this.usuario.cpfcnpj,
          email: this.usuario.email,
          telefone: this.usuario.telefone,
        }
        let response = await this.$axios.$post('http://localhost:3333/usuarios', usuario);
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
<template>
  <v-container>
    <h1>Consulta de usuarios</h1>
    <hr>
    <v-container style="background-color: white">
      <v-row>
        <v-col>
          <v-btn
            color="black"
            @click="getUsuarios"
          >
            Pesquisar 
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            style="margin-left: -70%"
            color="green"
            to="/usuarios/cadastro"
          >
            Cadastrar
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <v-data-table
        :headers="headers"
        :items="usuarios"
        :items-per-page="10"
        class="elevation-1"
      ></v-data-table>
    </v-container>
  </v-container>
</template>

<script>

export default {
  name: 'ConsultaUsuariosPage',
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
          text: 'Nome',
          align: 'center',
          sortable: false,
          value: 'nome'
        },
        {
          text: 'CPF / CNPJ',
          align: 'center',
          sortable: false,
          value: 'cpfcnpj'
        },
        {
          text: 'Email',
          align: 'center',
          sortable: false,
          value: 'email'
        },
        {
          text: 'Telefone',
          align: 'center',
          sortable: false,
          value: 'telefone'
        }
      ],
      usuarios: []
    }
  },

  created () {
    this.getUsuarios()
  },

  methods: {
    async getUsuarios () {
      this.usuarios = await this.$axios.$get('http://localhost:3333/usuarios')
    }
  }
}
</script>

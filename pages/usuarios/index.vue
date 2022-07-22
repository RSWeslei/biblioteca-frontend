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
      >
        <template v-slot:item.actions="{ item }">
          <v-icon
            small
            class="mr-2"
            @click="editar(item)"
          >
            mdi-pencil
          </v-icon>
          <v-icon
            small
            @click="deletar(item)"
          >
            mdi-delete
          </v-icon>
        </template>
      </v-data-table>
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
        },
        { 
          text: "",
          value: "actions" 
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
    },
    async deletar (usuario) {
      if (confirm(`Deseja deletar o usuario com id: ${usuario.id} e nome: ${usuario.nome}?`)) {
        try {
          let response = await this.$axios.$post('http://localhost:3333/usuarios/deletar', { id: usuario.id });
          this.$toast.success(response.message)
          this.getUsuarios();
        } catch (error) {
          this.$toast.error('Erro ao deletar o usuario')
        }
      }
    },
    async editar (usuario) {
      this.$router.push({
        name: 'usuarios-cadastro',
        params: { id: usuario.id }
      })
    }
  }
}
</script>

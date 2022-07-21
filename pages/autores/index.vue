<template>
  <v-container>
    <h1>Consulta de autores</h1>
    <hr>
    <v-container style="background-color: white">
      <v-row>
        <v-col>
          <v-btn
            color="black"
            @click="getAutores"
          >
            Pesquisar 
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            style="margin-left: -70%"
            color="green"
            to="/autores/cadastro"
          >
            Cadastrar
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <v-data-table
        :headers="headers"
        :items="autores"
        :items-per-page="10"  
        class="elevation-1"
      >
        <template v-slot:item.actions="{ item }">
          <v-icon
            small
            class="mr-2"
            @click="editItem(item)"
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
  name: 'ConsultaAutoresPage',
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
          value: 'nome'
        },
        {
          text: 'Email',
          align: 'center',
          sortable: false,
          value: 'email'
        },
        { 
          text: "",
          value: "actions" 
        }
      ],
      autores: []
    }
  },

  created () {
    this.getAutores()
  },

  methods: {
    async getAutores () {
      this.autores = await this.$axios.$get('http://localhost:3333/autores')
    },
    async deletar (autor) {
      if (confirm(`Deseja deletar o autor com id: ${autor.id} e nome: ${autor.nome}?`)) {
        try {
          let response = await this.$axios.$post('http://localhost:3333/autores/deletar', { id: autor.id });
          this.$toast.success(response.message)
          this.getAutores();
        } catch (error) {
          this.$toast.error('Erro ao deletar o autor')
        }
      }
    }
  }
}
</script>

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
      ></v-data-table>
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
          sortable: false,
          value: 'nome'
        },
        {
          text: 'Email',
          align: 'center',
          sortable: false,
          value: 'email'
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
    }
  }
}
</script>

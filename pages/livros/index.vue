<template>
  <v-container>
    <h1>Consulta de livros</h1>
    <hr>
    <v-container style="background-color: white">
      <v-row>
        <v-col>
          <v-btn
            color="black"
            @click="getLivros"
          >
            Pesquisar 
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            style="margin-left: -70%"
            color="green"
            to="/livros/cadastro"
          >
            Cadastrar
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <v-data-table
        :headers="headers"
        :items="livros"
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
  name: 'ConsultaLivrosPage',
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
          text: 'Titulo',
          align: 'center',
          value: 'titulo'
        },
        {
          text: 'Autor',
          align: 'center',
          sortable: false,
          value: 'autor.nome'
        },
        {
          text: 'Categoria',
          align: 'center',
          sortable: false,
          value: 'categoria.nome'
        },
        { 
          text: "Editar",
          value: "actions" 
        }
      ],
      livros: []
    }
  },

  created () {
    this.getLivros()
  },

  methods: {
    async getLivros () {
      this.livros = await this.$axios.$get('http://localhost:3333/livros')
    },

    async deletar (livro) {
      if (confirm(`Deseja deletar o livro com id: ${livro.id} e nome: ${livro.titulo}?`)) {
        try {
          let response = await this.$axios.$post('http://localhost:3333/livros/deletar', { id: livro.id });
          this.$toast.success(response.message)
          this.getLivros();
        } catch (error) {
          this.$toast.error('Erro ao deletar o livro')
        }
      }
    },
    async editar (livro) {
      this.$router.push({
        name: 'livros-cadastro',
        params: { id: livro.id }
      })
    }
  }
}
</script>

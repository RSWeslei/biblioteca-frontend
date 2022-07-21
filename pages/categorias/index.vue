<template>
  <v-container>
    <h1>Consulta de categorias</h1>
    <hr>
    <v-container style="background-color: white">
      <v-row>
        <v-col>
          <v-btn
            color="black"
            @click="getCategorias"
          >
            Pesquisar 
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            style="margin-left: -70%"
            color="green"
            to="/categorias/cadastro"
          >
            Cadastrar
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <v-data-table
        :headers="headers"
        :items="categorias"
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
  name: 'ConsultaCategoriasPage',
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
          text: "",
          value: "actions" 
        }
      ],
      categorias: []
    }
  },

  created () {
    this.getCategorias()
  },

  methods: {
    async getCategorias () {
      this.categorias = await this.$axios.$get('http://localhost:3333/categorias')
    },
    async deletar (categoria) {
      if (confirm(`Deseja deletar a categoria com id: ${categoria.id} e nome: ${categoria.nome}?`)) {
        try {
          let response = await this.$axios.$post('http://localhost:3333/categorias/deletar', { id: categoria.id });
          this.$toast.success(response.message)
          this.getCategorias();
        } catch (error) {
          this.$toast.error('Erro ao deletar a categoria')
        }
      }
    }
  }
}
</script>

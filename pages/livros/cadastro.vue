<template>
  <v-container>
    <h1>Cadastro de Livros</h1>
    <hr>
    <v-form>
      <v-container>
        <v-row>
          <v-col>
            <v-text-field
              v-model="livro.id"
              rounded
              placeholder="Código"
              outlined
              disabled
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="livro.titulo"
              label="Titulo"
              clearable
              outlined
              rounded
              placeholder="Titulo"
              color="green"
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="livro.sinopse"
              rounded
              placeholder="Sinopse"
              color="green"
              label="Sinopse"
              clearable
              outlined
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-autocomplete
              v-model="livro.idAutor"
              :items="autores"
              clearable
              outlined
              rounded
              placeholder="Autores"
              color="green"
              label="Autores"
              item-text="nome"
              item-value="id"
            ></v-autocomplete>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-autocomplete
              v-model="livro.idCategoria"
              :items="categorias"
              outlined
              rounded
              clearable
              placeholder="Categorias"
              color="green"
              label="Categorias"
              item-text="nome"
              item-value="id"
            ></v-autocomplete>
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
      to="/livros"
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
           {{ cadastroErro ? 'Erro ao cadastrar o livro' : 'Livro cadastrado com sucesso' }}
        </v-card-text>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroLivrosPage',

  data () {
    return {
      livro: {
        id: null,
        titulo: null,
        sinopse: null,
        idAutor: null,
        idCategoria: null
      },
      categorias: [],
      autores: [],
      cadastrado: false,
      cadastroErro: false
    }
  },

  created () {
    this.getAutores();
    this.getCategorias();
  },
  
  methods: {
    async cadastrar () {
      try {
        let livro = {
          titulo: this.livro.titulo,
          sinopse: this.livro.sinopse,
          idAutor: this.livro.idAutor,
          idCategoria: this.livro.idCategoria
        }
        let response = await this.$axios.$post('http://localhost:3333/livros', livro);
        console.log(response);
        this.cadastrado = true
      } catch (error) {
        this.cadastrado = true
        this.cadastroErro = true
      }
    },

    async getAutores () {
      this.autores = await this.$axios.$get('http://localhost:3333/autores');
    },

    async getCategorias () {
      this.categorias = await this.$axios.$get('http://localhost:3333/categorias');
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
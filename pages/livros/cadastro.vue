<template>
  <v-container>
    <h1>Cadastro de Livros</h1>
    <hr>
    <v-form v-model="valid">
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
              :rules="rule"
              required
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
              :rules="rule"
              required
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
              :rules="rule"
              required
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
              :rules="rule"
              required
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
  </v-container>
</template>

<script>
export default {
  name: 'CadastroLivrosPage',

  data () {
    return {
      valid: false,
      livro: {
        id: null,
        titulo: null,
        sinopse: null,
        idAutor: null,
        idCategoria: null
      },
      categorias: [],
      autores: [],
      rule: [
        v => !!v || 'Esse campo é obrigatório'
      ]
    }
  },

  created () {
    this.getAutores();
    this.getCategorias();
    if (this.$route?.params?.id){
      this.getById(this.$route?.params?.id)
    }
  },
  
  methods: {
    async cadastrar () {
      try {
        if (!this.valid){
          return this.$toast.warning('O formulário de cadastro não é válido!')
        }
        let livro = {
          titulo: this.livro.titulo,
          sinopse: this.livro.sinopse,
          idAutor: this.livro.idAutor,
          idCategoria: this.livro.idCategoria
        }

        if (!this.livro.id){
          await this.$axios.$post('http://localhost:3333/livros', livro);
          this.$toast.success('Livro cadastrado com sucesso')
          return this.$router.push('/livros')
        }
        await this.$axios.$post(`http://localhost:3333/livros/${this.livro.id}`, livro);
        this.$toast.success('Cadastro atualizado com sucesso!');
        return this.$router.push('/livros');
      } catch (error) {
        this.$toast.error('Erro ao cadastrar o livro')
      }
    },

    async getById (id) {
      this.livro = await this.$axios.$get(`http://localhost:3333/livros/${id}`);
    },

    async getAutores () {
      this.autores = await this.$axios.$get('http://localhost:3333/autores');
    },

    async getCategorias () {
      this.categorias = await this.$axios.$get('http://localhost:3333/categorias');
    }
  },
}
</script>

<style>
  .v-dialog {
    margin-top: 800px;
    margin-right: 800px;
  }
</style>
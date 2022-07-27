<template>
  <v-container>
    <h1>Visualizador de Empréstimos</h1>
    <hr>
    <v-form>
      <v-container class="main">
        <v-row>
          <v-col>
            <v-text-field
              rounded
              label="Código"
              :value="emprestimo.id"
              outlined
              disabled
            ></v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              rounded
              label="Usuario"
              :value="usuario ? usuario.nome : ''"
              outlined
              disabled
            ></v-text-field>
          </v-col>
        </v-row>
          <v-col>
            Prazo
          </v-col>
        <v-row>
          <v-col>
            <v-date-picker
              elevation="15"
              :value="emprestimo.prazo"
              color="green"
              locale="utc"
              title="Prazo"
              disabled
            ></v-date-picker>
          </v-col>
          <v-col>
            <v-data-table
              :headers="headers"
              :items="emprestimo ? emprestimo.livros : ''"
              :items-per-page="10"
              class="elevation-1"
              color="green"
            ></v-data-table>
          </v-col>
        </v-row>
      </v-container>
      <v-btn
        outlined
        to="/emprestimos"
      >
        Cancelar
      </v-btn>
    </v-form>
  </v-container>
</template>

<script>
export default {
  name: 'VisualizarEmprestimoPage',

  data () {
    return {
      emprestimo: {
        livros: [{
          titulo: null
        }]
      },
      usuario: null,
      headers: [
        {
          text: 'Livros do empréstimo',
          align: 'center',
          sortable: true,
          value: 'titulo'
        }
      ]
    }
  },

  async created () {
    try {
      if (this.$route?.params?.id){
        await this.getData(this.$route?.params?.id)
      }
      await this.getUsuarioById()
    } catch (error) {
      this.$toast.error('Erro ao visualizar o empréstimo')
    }

  },
  
  methods: {
    async getData (id) {
      this.emprestimo = await this.$axios.$get(`http://localhost:3333/emprestimos/${id}`); 
    },
    async getUsuarioById () {
      console.log(this.emprestimo.livros);
      this.usuario = await this.$axios.$get(`http://localhost:3333/usuarios/${this.emprestimo.idUsuario}`); 
    }
  }
}
</script>

<style>
  .main {
    background-image: url(https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRdGIriHwMYbjKJI76jDDK8KzXXSZhiNKGs9g&usqp=CAU);
    background-size: 80%;
    background-position: center;

  }
</style>
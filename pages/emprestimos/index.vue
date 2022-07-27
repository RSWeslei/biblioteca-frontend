<template>
  <v-container>
    <h1>Consulta de emprestimos</h1>
    <hr>
    <v-container style="background-color: white">
      <v-row>
        <v-col>
          <v-btn
            color="black"
            @click="getEmprestimos"
          >
            Pesquisar 
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            style="margin-left: -70%"
            color="green"
            to="/emprestimos/cadastro"
          >
            Cadastrar
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <v-data-table
        :headers="headers"
        :items="emprestimos"
        :items-per-page="10"
        class="elevation-1"
      >
        <template v-slot:item.actions="{ item }">
          <v-icon
            small
            @click="deletar(item)"
          >
            mdi-delete
          </v-icon>
          <v-icon
            style="margin-left: 20px"
            small
            @click="visualizar(item.id)"
          >
            mdi-eye
          </v-icon>
          <v-btn
            style="margin-left: 20px"
            color="red"
            @click="encerar(item)"
          >
            Encerar
          </v-btn>
        </template>
        <template v-slot:item.created_at="{ item }">
          <span>{{ new Date(item.created_at).toLocaleString() }}</span>
        </template>
        <template v-slot:item.prazo="{ item }">
          <span>{{ new Date(item.prazo).toLocaleString() }}</span>
        </template>
         <template v-slot:item.devolucao="{ item }">
          <span :style="!item.devolucao ? 'color: green' : null" >
            {{ item.devolucao ? new Date(item.devolucao).toUTCString(): 'Em progresso' }}
          </span>
        </template>
      </v-data-table>
    </v-container>
  </v-container>
</template>

<script>

export default {
  name: 'ConsultaEmprestimosPage',
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
          text: 'Prazo',
          align: 'center',
          sortable: false,
          value: 'prazo'
        },
        {
          text: 'Emprestado em:',
          align: 'center',
          sortable: false,
          value: 'created_at'
        },
        {
          text: 'Finalizado',
          align: 'center',
          sortable: true,
          value: 'devolucao'
        },
        { 
          text: "",
          value: "actions" 
        }
      ],
      emprestimos: []
    }
  },

  created () {
    this.getEmprestimos()
  },

  methods: {
    async getEmprestimos () {
      this.emprestimos = await this.$axios.$get('http://localhost:3333/emprestimos/')
    },
    async getUsuario () {
      
    },
    async deletar (item) {
       if (confirm(`Deseja deletar o empréstimo com id: ${item.id}?`)) {
        try {
          let response = await this.$axios.$post('http://localhost:3333/emprestimos/deletar', { id: item.id });
          this.$toast.success(response.message)
          this.getEmprestimos();
        } catch (error) {
          this.$toast.error('Erro ao deletar o empréstimo')
        }
      }
    },
    async encerar (item) {
      try {
        if (confirm(`Deseja encerar o empréstimo ${item.id}?`)){
          if (item.devolucao){
            return
          }
          let atualizado = {
            devolucao: new Date(Date.now()).toISOString(),
          }
          await this.$axios.$post(`http://localhost:3333/emprestimos/${item.id}`, atualizado);
          this.getEmprestimos()
        }
      } catch (error) {
        this.$toast.error('Erro ao encerar o empréstimo')
      }
    },
    visualizar (id) {
      this.$router.push({
        name: 'emprestimos-visualizar',
        params: { id: id }
      })
    }
  }
}
</script>

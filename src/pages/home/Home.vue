<template>
    <div>
      <div>
        <b-navbar toggleable="lg" type="dark" variant="info">
          <b-navbar-brand href="#">Origo</b-navbar-brand>

          <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

          <b-collapse id="nav-collapse" is-nav>
            <b-navbar-nav>
              <b-nav-item href="#">Link</b-nav-item>
              <b-nav-item href="#">Disabled</b-nav-item>
              <b-nav-item href="#">Link</b-nav-item>
              <b-nav-item href="#">Disabled</b-nav-item>
            </b-navbar-nav>
          </b-collapse>
        </b-navbar>
      </div>
      <div style="margin-bottom: 20px;" class="container-fluid">
        <table class="table">
          <thead>
            <tr style="text-align: center;">
              <th scope="col">Nome do Plano</th>
              <th scope="col">Valor do Plano</th>
            </tr>
          </thead>
          <tbody>
            <tr style="text-align: center;" v-for="(plano, idx) in planos" :key="idx">
              <td>{{ plano.nome }}</td>
              <td>R${{ plano.valorMensal }}</td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="container-fluid">
        <table class="table">
          <thead>
            <tr style="text-align: center;">
              <th scope="col">Nome</th>
              <th scope="col">Email</th>
              <th scope="col">Telefone</th>
              <th scope="col">Estado</th>
              <th scope="col">Cidade</th>
              <th scope="col">Data de Nascimento</th>
              <th scope="col">Editar</th>
              <th scope="col">Deletar</th>
            </tr>
          </thead>
          <tbody>
            <tr style="text-align: center;" v-for="(cliente, idx) in clientes" :key="idx">
              <td>{{ cliente.nome }}</td>
              <td>{{ cliente.email }}</td>
              <td>{{ cliente.telefone }}</td>
              <td>{{ cliente.estado }}</td>
              <td>{{ cliente.cidade }}</td>
              <td>{{ cliente.dataNascimento }}</td>
              <td>
                <b-button @click="editarCliente(cliente.id)" class="btn btn-warning" v-b-modal.modal-1>Editar</b-button>
              </td>
              <td>
                <button @click="deletarCliente(cliente.id)" type="button" class="btn btn-danger">Deletar</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>

      <div>
        <b-modal hide-footer id="modal-1" title="Editar Cliente">
          <div class="mb-3">
            <label for="exampleFormControlInput1" class="form-label">Nome</label>
            <input type="text" class="form-control" v-model="clienteEditar.nome">
            <label for="exampleFormControlInput1" class="form-label">Email</label>
            <input type="email" class="form-control" v-model="clienteEditar.email">
            <label for="exampleFormControlInput1" class="form-label">Telefone</label>
            <input type="text" class="form-control" v-model="clienteEditar.telefone">
            <label for="exampleFormControlInput1" class="form-label">Estado</label>
            <input type="text" class="form-control" v-model="clienteEditar.estado">
            <label for="exampleFormControlInput1" class="form-label">Cidade</label>
            <input type="text" class="form-control" v-model="clienteEditar.cidade">
            <label for="exampleFormControlInput1" class="form-label">Data de Nascimento</label>
            <input type="text" class="form-control" v-model="clienteEditar.dataNascimento">
          </div>
          <div>
            <b-button variant="warning" @click="hideModal1">Fechar</b-button>
            <b-button variant="success" @click="editarBotao(clienteEditar.id)">Editar</b-button>
          </div>

        </b-modal>
      </div>
    </div>
</template>

<script>

export default {
  name: 'Home',
  data () {
    return {
      clientes: [],
      planos: [],
      clienteEditar: {}
    }
  },
  methods: {
    getPlanos() {
      this.axios.get('planos')
      .then(res => {
        this.planos = res.data
      })
      .catch(e => {
        alert(e.response.data.name + ': ' + e.response.data.message)
      })
    },
    getClientes() {
      this.axios.get('clientes')
      .then(res => {
        this.clientes = res.data
      })
      .catch(e => {
        alert(e.response.data.name + ': ' + e.response.data.message)
      })
    },
    editarCliente(id) {
      this.axios.get('clientes/' + id)
      .then(res => {
        this.clienteEditar = res.data
      })
      .catch(e => {
        alert(e.response.data.name + ': ' + e.response.data.message)
      })
    },
    editarBotao(id) {
      this.axios.put('clientes/' + id, this.clienteEditar)
      .then(res => {
        alert('Cliente ' + res.data.nome + ' atualizado com sucesso')
        this.hideModal()
      })
      .catch(e => {
        alert(e.response.data.name + ': ' + e.response.data.message)
      })
    },
    deletarCliente(id) {
      this.axios.delete('clientes/' + id)
      .then(res => {
        alert('Cliente ' + res.data.nome + ' deletado com sucesso')
        this.getClientes()
      })
      .catch(e => {
        alert(e.response.data.name + ': ' + e.response.data.message)
      })
    },
    hideModal1() {
      this.$root.$emit('bv::hide::modal', 'modal-1', '#btnShow')
    },
  },
  mounted () {
    this.getClientes()
    this.getPlanos()
  }
}
</script>

<style scoped>
  #topo{
    background-color: #87CEEB;
    margin-bottom: 100px;
  }

  h2{
    text-align: center;
    color: white;
  }

  .branco{
    text-align: center;
    color: white;
  }

  #btn-enviar{
    background-color: #87CEEB;
    border-color: white;
  }

  h3, p{
    text-align: center;
  }

</style>
<template>
  <div class="col-lg-6">

    <!-- Modal -->
    <div
      class="modal fade"
      ref="incomingModal"
      id="incomingModal"
      tabindex="-1"
      aria-labelledby="incomingModalLabel"
      aria-hidden="true">
      <div
        class="modal-dialog">
        <div
          class="modal-content">
          <div
            class="modal-header">
            <h5
              class="modal-title"
              id="incomingModalLabel">
              Criar Receita
            </h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close">
            </button>
          </div>
          <div class="modal-body">
            <form>
              <div class="form-group">
                <label for="exampleFormControlSelect1">Tipo</label>
                <select
                  v-model="newIncoming.type"
                  class="form-control">
                  <option
                    v-for="type in types"
                    :value="type.id"
                    :key="type.id">
                    {{ type.description }}
                  </option>
                </select>
              </div>

              <div class="form-group mt-4">
                <label>Valor</label>

                <div class="input-group">
                  <div class="input-group-prepend">
                    <span
                      class="input-group-text">
                      Kz
                    </span>
                  </div>
                  <input
                    v-model="newIncoming.value"
                    type="number"
                    class="form-control"
                    placeholder="Valor da receita" />
                </div>
              </div>

              <div class="form-group mt-4">
                <label>Descrição</label>
                <textarea
                  v-model="newIncoming.description"
                  class="form-control"
                  rows="3">
                </textarea>
              </div>

              <div class="form-group mt-4">
                <label>Data</label>
                <input
                  v-model="newIncoming.date"
                  type="date"
                  class="form-control"
                  placeholder="Data" />
              </div>
            </form>
          </div>

          <div class="modal-footer">
            <button
              type="button"
              ref="closeBtn"
              class="btn btn-secondary"
              data-bs-dismiss="modal">
              Fechar
            </button>

            <button
              @click="addIncoming()"
              type="button"
              class="btn btn-primary">
              Guardar
            </button>
          </div>
        </div>
      </div>
    </div>

    <div class="table-title">
      <h2>Rendimentos</h2>
      <button
        type="button"
        class="btn btn-outline-success"
        data-bs-toggle="modal" data-bs-target="#incomingModal">
        Adicionar Rendimento
      </button>
    </div>

    <table class="table table-striped">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Tipo</th>
          <th scope="col">Valor</th>
          <th scope="col">Descrição</th>
          <th scope="col">Data</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-if="!incomings.length">
          <td colspan="5">Não existem receitas</td>
        </tr>
        <tr
          v-else
          v-for="(incoming, index) in incomings"
          :key="index">
          <th>{{ index + 1 }}</th>
          <td>{{ getTypeDescription(incoming.type) }}</td>
          <td>{{ incoming.value }}</td>
          <td>{{ incoming.description }}</td>
          <td>{{ incoming.date }}</td>
        </tr>        
      </tbody>
    </table>
  </div>
</template>

<script>
import _ from 'lodash'

export default {
  name: 'Incomings',

  props: {
    incomings: {
      type: Array,
      required: true
    },

    types: {
      type: Array,
      required: true
    }
  },

  components: {
  },

  computed: {
  },

  data () {
    return {
      newIncoming: {
        type: null,
        value: 0,
        date: null,
        description: ''
      }
    }
  },

  methods: {
    addIncoming () {
      this.$emit('add-incoming', this.newIncoming)

      this.newIncoming = {
        type: null,
        value: 0,
        date: null,
        description: ''
      }

      this.$refs.closeBtn.click()
    },

    getTypeDescription (id) {
      return _.find(this.types, type => {
        return type.id == id
      }).description
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

.table-title {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}
</style>

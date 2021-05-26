<template>
  <div class="container">
    <ul class="nav nav-tabs" id="myTab" role="tablist">
      <li class="nav-item" role="presentation">
        <button 
          class="nav-link active" 
          id="general-tab" 
          data-bs-toggle="tab" 
          data-bs-target="#general" 
          type="button" 
          role="tab" 
          aria-controls="general" 
          aria-selected="true">
          Visão Geral
        </button>
      </li>

      <li 
        class="nav-item" 
        role="presentation">
        <button 
          class="nav-link" 
          id="incomings-by-month-tab" 
          data-bs-toggle="tab" 
          data-bs-target="#incomings-by-month" 
          type="button" 
          role="tab" 
          aria-controls="incomings-by-month" 
          aria-selected="false">
          Receitas / mês
        </button>
      </li>

      <li 
        class="nav-item" 
        role="presentation">
        <button 
          class="nav-link" 
          id="expenses-by-month-tab" 
          data-bs-toggle="tab" 
          data-bs-target="#expenses-by-month" 
          type="button" 
          role="tab" 
          aria-controls="expenses-by-month" 
          aria-selected="false">
          Despesas / mês
        </button>
      </li>

      <li 
        class="nav-item" 
        role="presentation">
        <button 
          class="nav-link" 
          id="incomings-by-type-tab" 
          data-bs-toggle="tab" 
          data-bs-target="#incomings-by-type" 
          type="button" 
          role="tab" 
          aria-controls="incomings-by-type" 
          aria-selected="false">
          Receitas / tipo
        </button>
      </li>

      <li 
        class="nav-item" 
        role="presentation">
        <button 
          class="nav-link" 
          id="expenses-by-type-tab" 
          data-bs-toggle="tab" 
          data-bs-target="#expenses-by-type" 
          type="button" 
          role="tab" 
          aria-controls="expenses-by-type" 
          aria-selected="false">
          Despesas / tipo
        </button>
      </li>
    </ul>

    <div class="tab-content" id="myTabContent">
      <div 
        class="tab-pane fade show active" 
        id="general" 
        role="tabpanel" 
        aria-labelledby="general-tab">

        <div class="row mt-4">
          <div class="col-lg-4">
            <div class="card  mb-4 box-shadow">
              <div class="card-header bg-success text-white">
                <h4 class="my-0 font-weight-normal">Receitas</h4>
              </div>
              <div class="card-body">
                <h1 class="card-title pricing-card-title">{{ formatMoney(incomingsTotal) }}</h1>      
              </div>
            </div>
          </div>

          <div class="col-lg-4">
            <div class="card mb-4 box-shadow">
              <div class="card-header bg-danger text-white">
                <h4 class="my-0 font-weight-normal">Despesas</h4>
              </div>
              <div class="card-body">
                <h1 class="card-title pricing-card-title">{{ formatMoney(expensesTotal) }}</h1>      
              </div>
            </div>
          </div>

          <div class="col-lg-4">
            <div class="card mb-4 box-shadow">
              <div class="card-header bg-primary text-white">
                <h4 class="my-0 font-weight-normal">Saldo</h4>
              </div>
              <div class="card-body">
                <h1 class="card-title pricing-card-title">{{ formatMoney(balance) }}</h1>      
              </div>
            </div>
          </div>
        </div>

        <div class="row mt-4">
          <Incomings 
            :incomings="incomings"
            :types="incomingTypes"
            @add-incoming="addIncoming" />
          <Expenses 
            :expenses="expenses"
            :types="expenseTypes"
            @add-expense="addExpense" />
        </div>
      </div>
      
      <div 
        class="tab-pane fade" 
        id="incomings-by-month" 
        role="tabpanel" 
        aria-labelledby="incomings-by-month-tab">
        <div
          class="row mt-4">
          <div class="col-lg-12">
            <bar-chart
              :height="70"
              ref="incomings"
              :chartData="incomingsData"
              :options="options">
            </bar-chart>
          </div>  
        </div>
      </div>

      <div 
        class="tab-pane fade" 
        id="expenses-by-month" 
        role="tabpanel" 
        aria-labelledby="expenses-tab-by-month">
        <div
          class="row mt-4">
          <div class="col-lg-12">
            <bar-chart
              :height="70"
              ref="expenses"
              :chartData="expensesData"
              :options="options">
            </bar-chart>
          </div>  
        </div>
      </div>

      <div 
        class="tab-pane fade" 
        id="incomings-by-type" 
        role="tabpanel" 
        aria-labelledby="incomings-by-type-tab">
        <div
          class="row mt-4">
          <div class="col-lg-12">
            <table class="table table-striped">
              <thead>
                <tr>
                  <th scope="col">#</th>
                  <th scope="col">Tipo</th>
                  <th scope="col">Valor</th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-if="!incomingsByType.length">
                  <td colspan="5">Não existem receitas</td>
                </tr>
                <tr
                  v-else
                  v-for="(incomingByType, index) in incomingsByType"
                  :key="index">
                  <th>{{ index + 1 }}</th>
                  <td>{{ getIncomingTypeDescription(incomingByType.type) }}</td>
                  <td>{{ incomingByType.value }}</td>
                </tr>        
              </tbody>
            </table>
          </div>
        </div>
      </div>

      <div 
        class="tab-pane fade" 
        id="expenses-by-type" 
        role="tabpanel" 
        aria-labelledby="expenses-tab-by-type">
        <div
          class="row mt-4">
          <div class="col-lg-12">
            <table class="table table-striped">
              <thead>
                <tr>
                  <th scope="col">#</th>
                  <th scope="col">Tipo</th>
                  <th scope="col">Valor</th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-if="!expensesByType.length">
                  <td colspan="5">Não existem despesas</td>
                </tr>
                <tr
                  v-else
                  v-for="(expenseByType, index) in expensesByType"
                  :key="index">
                  <th>{{ index + 1 }}</th>
                  <td>{{ getExpenseTypeDescription(expenseByType.type) }}</td>
                  <td>{{ expenseByType.value }}</td>
                </tr>        
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Expenses from '@/components/Expenses.vue'
import Incomings from '@/components/Incomings.vue'
import BarChart from '@/directives/barChart'
import _ from 'lodash'

export default {
  name: 'Dashboard',

  components: {
    Expenses,
    Incomings,
    BarChart
  },

  computed: {
    incomingsTotal () {
      return _.sumBy(this.incomings, incoming => {
        return parseFloat(incoming.value)
      })
    },

    expensesTotal () {
      return _.sumBy(this.expenses, expense => {
        return parseFloat(expense.value)
      })
    },

    balance () {
      return this.incomingsTotal - this.expensesTotal
    }
  },

  data () {
    return {
      incomings: [],

      expenses: [],

      incomingTypes: [
        {
          id: 1,
          description: 'Salário',
        },
        {
          id: 2,
          description: 'Renda',
        },
        {
          id: 3,
          description: 'Investimentos',
        },
        {
          id: 4,
          description: 'Outro',
        }
      ],

      expenseTypes: [
        {
          id: 1,
          description: 'Água',
        },
        {
          id: 2,
          description: 'Luz',
        },
        {
          id: 3,
          description: 'Alimentação',
        },
        {
          id: 4,
          description: 'Outro',
        }
      ],

      incomingsByType: [],

      expensesByType: [],

      incomingsData: {
          labels: [],
          datasets: [
              {
                  label: 'Despesas por mês',
                  backgroundColor: 'rgba(213, 43, 30, 0.90)',
                  pointBorderColor: '#fff',
                  data: []
              }
          ]
      },

      expensesData: {
        labels: [],
        datasets: [
          {
              label: 'Despesas por mês',
              backgroundColor: 'rgba(213, 43, 30, 0.90)',
              pointBorderColor: '#fff',
              data: []
          }
        ]
      },

      options: {
          legend: {
              display: false
          },
          
          scales: {
              yAxes: [
                  {
                      ticks: {
                          callback: function(value) {
                              return value + ' Kz';
                          },
                          beginAtZero: true
                      },
                      gridLines: {
                          color: "rgba(116, 119, 121, 0.10)"
                      }
                  }
              ],
              xAxes: [
                  {
                      gridLines: {
                          display: false
                      }
                  }
              ]
          },
          animation: {
              easing: 'linear'
          },
          responsive: true       
      }
    }
  },

  methods: {
    processExpensesData () {
      var sortedExpenses = _.sortBy(this.expenses, (expense) => {
        var date = new Date(expense.date)

        return date
      })

      // By month

      let months = _.map(sortedExpenses, expense => {
        var date = new Date(expense.date)

        return date.getUTCMonth() + 1 + '/' + date.getUTCFullYear()
      })

      let uniqueMonths = _.uniq(months)

      this.expensesData.labels = uniqueMonths

      var expensesByMonth = []

      _.forEach(uniqueMonths, uniqueMonth => {
        let totalByMonth = _.sumBy(sortedExpenses, expense => {
          var date = new Date(expense.date)
          
          if ((date.getUTCMonth() + 1 + '/' + date.getUTCFullYear()) == uniqueMonth) {
            return parseFloat(expense.value)
          }
        })

        expensesByMonth.push(parseFloat(totalByMonth))
      })

      this.expensesData.datasets[0].data = expensesByMonth

      this.$refs.expenses.renderChart(this.expensesData, this.options)

      // By type

      let categories = _.map(sortedExpenses, expense => {
        return expense.type
      })

      let uniqueTypes = _.uniq(categories)

      var expensesByType = []

      _.forEach(uniqueTypes, uniqueType => {
        let totalByType = _.sumBy(sortedExpenses, expense => {          
          if (expense.type == uniqueType) {
            return parseFloat(expense.value)
          }
        })

        expensesByType.push(
          {
            type: uniqueType,
            value: parseFloat(totalByType)
          }
        )
      })

      this.expensesByType = expensesByType
    },

    processIncomingsData () {
      var sortedIncomings = _.sortBy(this.incomings, (incoming) => {
        var date = new Date(incoming.date)

        return date
      })

      // By month

      let months = _.map(sortedIncomings, incoming => {
        var date = new Date(incoming.date)

        return date.getUTCMonth() + 1 + '/' + date.getUTCFullYear()
      })

      let uniqueMonths = _.uniq(months)

      this.incomingsData.labels = uniqueMonths

      var incomingsByMonth = []

      _.forEach(uniqueMonths, uniqueMonth => {
        let totalByMonth = _.sumBy(sortedIncomings, incoming => {
          var date = new Date(incoming.date)
          
          if ((date.getUTCMonth() + 1 + '/' + date.getUTCFullYear()) == uniqueMonth) {
            return parseFloat(incoming.value)
          }
        })

        incomingsByMonth.push(parseFloat(totalByMonth))
      })

      this.incomingsData.datasets[0].data = incomingsByMonth

      this.$refs.incomings.renderChart(this.incomingsData, this.options)

      // By type

      let categories = _.map(sortedIncomings, incoming => {
        return incoming.type
      })

      let uniqueTypes = _.uniq(categories)

      var incomingsByType = []

      _.forEach(uniqueTypes, uniqueType => {
        let totalByType = _.sumBy(sortedIncomings, incoming => {          
          if (incoming.type == uniqueType) {
            return parseFloat(incoming.value)
          }
        })

        incomingsByType.push(
          {
            type: uniqueType,
            value: parseFloat(totalByType)
          }
        )
      })

      this.incomingsByType = incomingsByType
    },

    addIncoming (incoming) {
      this.incomings.push(incoming)

      this.processIncomingsData()
    },

    addExpense (expense) {
      this.expenses.push(expense)

      this.processExpensesData()
    },

    formatMoney (value) {
      return new Intl.NumberFormat('pt-AO', { style: 'currency', currency: 'AOA' }).format(value)
    },

    getIncomingTypeDescription (id) {
      return _.find(this.incomingTypes, type => {
        return type.id == id
      }).description
    },

    getExpenseTypeDescription (id) {
      return _.find(this.expenseTypes, type => {
        return type.id == id
      }).description
    }
  },

  mounted () {
  }
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

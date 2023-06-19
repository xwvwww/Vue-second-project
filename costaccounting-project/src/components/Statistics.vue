<template>
  <div>
    <h2>Статистика</h2>
    <p>Общие расходы по категориям:</p>
    <ul>
      <li v-for="(expense, category) in expensesByCategory">{{ category }}: {{ expense }}</li>
    </ul>
    <p>Самые большие расходы за последний месяц:</p>
    <ul>
      <li v-for="expense in largestExpenses">{{ expense.amount }} on {{ expense.date }} for {{ expense.category }}</li>
    </ul>
    <p>Окончательный баланс доходов и расходов: {{ finalBalance }}</p>
    <button @click="clearStatistics">Очистить статистику</button>
  </div>
</template>

<script>
export default {
  name: 'Statistics',
  props: ['expenses', 'incomes'],
  computed: {
    expensesByCategory() {
      const categories = {}
      this.expenses.forEach(expense => {
        if (!categories[expense.category]) {
          categories[expense.category] = 0
        }
        categories[expense.category] += expense.amount
      })
      return categories
    },
    largestExpenses() {
      const lastMonth = new Date()
      lastMonth.setMonth(lastMonth.getMonth() - 1)
      return this.expenses
        .filter(expense => {
          const expenseDate = new Date(expense.date)
          return expenseDate >= lastMonth
        })
        .sort((a, b) => b.amount - a.amount)
        .slice(0, 3)
    },
    finalBalance() {
      let expensesTotal = 0
      let incomesTotal = 0
      this.expenses.forEach(expense => {
        expensesTotal += expense.amount
      })
      this.incomes.forEach(income => {
        incomesTotal += income.amount
      })
      return incomesTotal - expensesTotal
    }
  },
  methods: {
    clearStatistics() {
      this.$emit('clear-statistics')
    }
  }
}
</script>

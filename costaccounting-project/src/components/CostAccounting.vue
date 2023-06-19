<template>
    <div class="cost-accounting">
      <h2>Учет затрат</h2>
      <div class="cost-accounting__header">
        <h3>Добавить расходы</h3>
        <div>
          <label for="amount">Сумма:</label>
          <input type="number" id="amount" v-model="expenseAmount">
        </div>
        <div>
          <label for="date">Дата:</label>
          <input type="date" id="date" v-model="expenseDate">
        </div>
        <div>
          <label for="category">Категория</label>
          <select id="category" v-model="selectedCategory">
            <option value="">Выберите категорию</option>
            <option value="food">Еда</option>
            <option value="clothing">Одежда</option>
            <option value="entertainment">Развлечение</option>
            <option value="appliances">Техника</option>
          </select>
        </div>
        <div>
          <label for="comment">Комментария:</label>
          <textarea id="comment" v-model="expenseComment"></textarea>
        </div>
        <button @click="addExpense">Добавить расход</button>
      </div>
      <div class="cost-accounting__header">
        <h3>Добавить доход</h3>
        <div>
          <label for="amount">Сумма:</label>
          <input type="number" id="amount" v-model="incomeAmount">
        </div>
        <div>
          <label for="date">Дата:</label>
          <input type="date" id="date" v-model="incomeDate">
        </div>
        <div>
          <label for="source">Источник:</label>
          <input type="text" id="source" v-model="incomeSource">
        </div>
        <button @click="addIncome">Добавить доход</button>
      </div>
      <h3>Статиститка</h3>
      <div class="cost-accounting__statistics">
        <h4>Общие расходы по категориям:</h4>
        <ul>
          <li v-for="(expense, index) in expensesByCategory" :key="index">
            {{ expense.category }}: {{ expense.total }}
          </li>
        </ul>
        <h4>Самые большие расходы за последний месяц:</h4>
        <ol>
          <li v-for="(expense, index) in largestExpenses" :key="index">
            {{ expense.amount }} on {{ expense.date }} for {{ expense.category }}
          </li>
        </ol>
        <h4>Окончательный баланс: {{ finalBalance }}</h4>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        expenses: [],
        incomes: [],
        expenseAmount: 0,
        expenseDate: '',
        selectedCategory: '',
        expenseComment: '',
        incomeAmount: 0,
        incomeDate: '',
        incomeSource: ''     };
  },
  computed: {
    expensesByCategory() {
      const categories = {};
      this.expenses.forEach(expense => {
        if (!categories[expense.category]) {
          categories[expense.category] = {
            category: expense.category,
            total: 0
          };
        }
        categories[expense.category].total += expense.amount;
      });
      return Object.values(categories);
    },
    largestExpenses() {
      const lastMonthExpenses = this.expenses.filter(expense => {
        const expenseMonth = new Date(expense.date).getMonth();
        const currentMonth = new Date().getMonth();
        return expenseMonth === currentMonth;
      });
      return lastMonthExpenses
        .sort((a, b) => b.amount - a.amount)
        .slice(0, 3);
    },
    finalBalance() {
      let expensesTotal = 0;
      let incomeTotal = 0;
      this.expenses.forEach(expense => {
        expensesTotal += expense.amount;
      });
      this.incomes.forEach(income => {
        incomeTotal += income.amount;
      });
      return incomeTotal - expensesTotal;
    }
  },
  methods: {
    addExpense() {
      this.expenses.push({
        amount: this.expenseAmount,
        date: this.expenseDate,
        category: this.selectedCategory,
        comment: this.expenseComment
      });
      this.expenseAmount = 0;
      this.expenseDate = '';
      this.selectedCategory = '';
      this.expenseComment = '';
      this.saveData();
    },
    addIncome() {
      this.incomes.push({
        amount: this.incomeAmount,
        date: this.incomeDate,
        source: this.incomeSource
      });
      this.incomeAmount = 0;
      this.incomeDate = '';
      this.incomeSource = '';
      this.saveData();
    },
    saveData() {
      localStorage.setItem('expenses', JSON.stringify(this.expenses));
      localStorage.setItem('incomes', JSON.stringify(this.incomes));
    }
  },
  mounted() {
    const expensesData = localStorage.getItem('expenses');
    if (expensesData) {
      this.expenses = JSON.parse(expensesData);
    }
    const incomesData = localStorage.getItem('incomes');
    if (incomesData) {
      this.incomes = JSON.parse(incomesData);
    }
  }
};
</script>

<style>
.cost-accounting {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}
.cost-accounting__header {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}
.cost-accounting__statistics {
  display: flex;
  flex-direction: column;
}
</style>

  
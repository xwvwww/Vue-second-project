<template>
    <div>
      <h2>Расход</h2>
      <form @submit.prevent="addExpense">
        <div>
          <label>Сумма:</label>
          <input type="number" v-model.number="amount" />
        </div>
        <div>
          <label>Дата:</label>
          <input type="date" v-model="date" />
        </div>
        <div>
          <label>Категория</label>
          <select v-model="category">
            <option value="food">Еда</option>
            <option value="clothing">Одежда</option>
            <option value="entertainment">Развлечение</option>
            <option value="appliances">Техника</option>
          </select>
        </div>
        <div>
          <label>Комментария:</label>
          <input type="text" v-model="comment" />
        </div>
        <button type="submit">Добавить расход</button>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    name: 'Expense',
    data() {
      return {
        amount: 0,
        date: '',
        category: 'food',
        comment: ''
      }
    },
    methods: {
      addExpense() {
        this.$emit('add-expense', {
          amount: this.amount,
          date: this.date,
          category: this.category,
          comment: this.comment
        })
        this.amount = 0
        this.date = ''
        this.category = 'food'
        this.comment = ''
      }
    }
  }
  </script>
  
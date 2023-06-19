<template>
    <div>
      <h2>Доход</h2>
      <form @submit.prevent="addIncome">
        <div>
          <label>Сумма:</label>
          <input type="number" v-model.number="amount" />
        </div>
        <div>
          <label>Дата:</label>
          <input type="date" v-model="date" />
        </div>
        <div>
          <label>Источник</label>
          <input type="text" v-model="source" />
        </div>
        <button type="submit">Добавить доход</button>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    name: 'Income',
    data() {
      return {
        amount: 0,
        date: '',
        source: ''
      }
    },
    methods: {
      addIncome() {
        this.$emit('add-income', {
          amount: this.amount,
          date: this.date,
          source: this.source
        })
        this.amount = 0
        this.date = ''
        this.source = ''
      }
    }
  }
  </script>
  
<template>
  <Header />
  <div class="container">
    <Balance :total="+total"/>
    <IncomeExpenses :income="+income" :expense="+expense"/>
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionsDeleted" />
    <AddTransaction @transactionsSubmitted="handleTransactionsSubmitted" />
  </div>
</template>

<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

import { ref, computed} from "vue";

const transactions = ref([
  { id: 1, text: "Flower", amount: -19.99 },
  { id: 2, text: "Salary", amount: 299.97 },
  { id: 3, text: "Book", amount: -10 },
  { id: 4, text: "Camera", amount: 150 },
]);
// get Total
const total = computed(() => {
  return transactions.value.reduce((acc, item) => (acc += item.amount), 0).toFixed(2);
});

// get Income
 const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, item) => (acc += item.amount), 0)
    .toFixed(2);
  });
// get Expense
const expense = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, item) => (acc += item.amount), 0)
    .toFixed(2);
});
// add transaction
const handleTransactionsSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });
};
// delete transaction
const handleTransactionsDeleted = (id) => {
  transactions.value = transactions.value.filter(
    (transaction)=> transaction.id !== id);
};
// generate unique id
const generateUniqueId = () => {
  return Math.floor(Math.random() * 100000000);
};
</script>
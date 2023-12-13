<template>
  <Header />
  <div class="container">
    <Balance :totalBalance="+totalBalance" />
    <IncomeExpenses :income="+income" :expense="+expense" />
    <TranscationLists :transactions="transactions" @deleteSubmit="deleteItem" />
    <AddTranscation @transcationSubmitted="handleAddSubmit" />
  </div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TranscationLists from './components/TranscationLists.vue';
import AddTranscation from './components/AddTranscation.vue';
import { ref, computed,onMounted } from 'vue';


const transactions = ref([
])

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});

//total balance
const totalBalance = computed(() => {
  return transactions.value.reduce((pv, cv) => {
    return pv + cv.amount
  }, 0)
});

//income
const income = computed(() => {
  return transactions.value.filter((transaction) => transaction.amount > 0)
    .reduce((pv, cv) => {
      return pv + cv.amount
    }, 0)
});

//expenses
const expense = computed(() => {
  return transactions.value.filter((transaction) => transaction.amount < 0)
    .reduce((pv, cv) => {
      return pv + cv.amount
    }, 0)
});

//handle add transcation

const handleAddSubmit = (transcationData) => {
  transactions.value.push({
    id: uniqueId(),
    text: transcationData.text,
    amount: transcationData.amount
  });
  saveTransactionsToLocalStorage();
}

const uniqueId = () => {
  return Math.floor(Math.random() * 1000);
}

//delete transcation
const deleteItem = (id) => {
  return transactions.value = transactions.value.filter(tran => {
    return tran.id !== id;
  });
  saveTransactionsToLocalStorage();


}
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
};




</script>
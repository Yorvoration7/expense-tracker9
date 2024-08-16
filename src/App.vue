<template>
  <div>
<Header/>
<div class="container">
  <Balance :total="total"/>
  <IncomeExpensesList :income="+income" :expenses="-expenses"/>
  <TransactionList :transactions="transactions" 
  @transactionDeleted="handleTransactionDeleted"/>
  <AddTransaction 
  @TransactionSubmitted="handleTransactionSubmitted"/>
</div>
  </div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpensesList from './components/IncomeExpensesList.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import {useToast} from 'vue-toastification';
import {ref, computed, onMounted} from 'vue';

const toast = useToast();

const transactions = ref([]);
onMounted (() => {
  const savedTransactions = JSON.parse(localStorage.getItem
  ('transactions'));

if (savedTransactions) {
  transactions.value = savedTransactions;
}


})

console.log(transactions.value);
// Get total
const total = computed(() => {
  return transactions.value.reduce((acc,transaction) => {
return acc + transaction.amount
  }, 0)
}) 
// Get Income
const income = computed(() => {
  return transactions.value
  .filter((transaction) => transaction.amount > 0)
  .reduce((acc,transaction) => {
return acc + transaction.amount
}, 0).toFixed(2)
}) 

//Get expenses
const expenses = computed(() => {
  return transactions.value
  .filter((transaction) => transaction.amount < 0)
  .reduce((acc,transaction) => {
return acc + transaction.amount
  }, 0).toFixed(2)
}) 


// ADD TRANSACTION 

const handleTransactionSubmitted = (transactionData) => {
transactions.value.push({
  id:generateUniqueId(),
  text: transactionData.text,
  amount: transactionData.amount,
})
savedTransactionsToLocalStorage();

toast.success('Transaction added successfully')
}

// generateUniqueId

const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000 )  
}
// DELETE TRANSACTION
const handleTransactionDeleted = (id) => {

  transactions.value = transactions.value.filter((transaction) =>
   transaction.id !== id);

savedTransactionsToLocalStorage();
toast.success('Transaction deleted');
console.log(id);
}
// save to local storage

const savedTransactionsToLocalStorage = () => {
localStorage.setItem('transactions', JSON.stringify(transactions.value))
}
</script>

<style lang="scss" scoped>

</style>
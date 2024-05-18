<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpense :income="+income" :expense="+expense" />

    <TransactionList :transactions="transactions" />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>

<script setup>
import Balance from "./components/Balance.vue";
import Header from "./components/Header.vue";
import IncomeExpense from "./components/IncomeExpense.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { useToast } from "vue-toastification";
import { ref, computed } from "vue";

const toast = useToast();
const transactions = ref([
  { id: 1, text: "Flower", amount: -20 },
  { id: 2, text: "Salary", amount: 300 },
  { id: 3, text: "Book", amount: -10 },
  { id: 4, text: "Camera", amount: 150 },
]);

// Get total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

//  get Income
const income = computed(() => {
  const filteredTransactions = transactions.value.filter(
    (transaction) =>
      typeof transaction.amount === "number" && transaction.amount > 0
  );
  return filteredTransactions.length > 0
    ? filteredTransactions
        .reduce((acc, transaction) => acc + transaction.amount, 0)
        .toFixed(2)
    : "0.00";
});

// Get Expenses
const expense = computed(() => {
  const filteredTransactions = transactions.value.filter(
    (transaction) =>
      typeof transaction.amount === "number" && transaction.amount < 0
  );
  return filteredTransactions.length > 0
    ? filteredTransactions
        .reduce((acc, transaction) => acc + transaction.amount, 0)
        .toFixed(2)
    : "0.00";
});

// Generate unique ID
const generateUniqueId = () => {
  return Math.floor(Math.random() * 100000);
};

// Adding transaction
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });

  toast.success("Transaction added successfully");
};
</script>

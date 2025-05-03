<script setup>
import { ref, onMounted } from 'vue';

const days = ['Senin', 'Selasa', 'Rabu', 'Kamis', 'Jumat', 'Sabtu', 'Minggu'];
const selectedDay = ref('Minggu');
const filter = ref('all');
const expenses = ref({});
const newExpense = ref({
  text: '',
  amount: 0,
  completed: false
});

days.forEach(day => {
  expenses.value[day] = [];
});

//fungsi menambah jenis pengeluaran dan jumlah pengeluaran 
function addExpense() {
  if (!newExpense.value.text || newExpense.value.amount <= 0) return;
  
  expenses.value[selectedDay.value].push({
    ...newExpense.value,
    id: Date.now()
  });

  newExpense.value = { text: '', amount: 0, completed: false };
  saveToLocalStorage();
}

function removeExpense(day, index) {
  expenses.value[day].splice(index, 1);
  saveToLocalStorage();
}

function filteredExpenses(day) {
  const dayExpenses = expenses.value[day];
  return filter.value === 'uncompleted' 
    ? dayExpenses.filter(exp => !exp.completed)
    : dayExpenses;
}




</script>

<template>
 <div class="expense-tracker">
    <h1>Pengeluaran Anak Kos</h1>
    
    <div class="controls">
      <select v-model="selectedDay">
        <option v-for="day in days" :value="day">{{ day }}</option>
      </select>
      <input 
        v-model="newExpense.text" 
        placeholder="Misal: Makan siang..."
        @keyup.enter="addExpense"
      >
      <input
        v-model.number="newExpense.amount"
        type="number"
        placeholder="Rp"
      >
      <button @click="addExpense">Tambah</button>
    </div>
    <div class="filter-buttons">
      <button @click="filter = 'all'" :class="{ active: filter === 'all' }">Semua</button>
      <button @click="filter = 'uncompleted'" :class="{ active: filter === 'uncompleted' }">Belum Dibayar</button>
    </div>

    <div v-for="day in days" :key="day" class="day-section">
      <h2>{{ day }}</h2>
      <ul>
        <li 
          v-for="(expense, index) in filteredExpenses(day)" 
          :key="index"
          :class="{ completed: expense.completed }"
        >
          <input 
            type="checkbox" 
            v-model="expense.completed"
            @change="saveToLocalStorage"
          >
        </li>
      </ul>
  </div>
  </div>
</template>
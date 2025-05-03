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
  </div>
</template>
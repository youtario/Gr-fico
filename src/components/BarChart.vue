<script setup>
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'
import { Bar } from 'vue-chartjs'

// Registrar os módulos do Chart.js
ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

// Dados do gráfico
const chartData = {
  labels: ['Janeiro', 'Fevereiro', 'Março'],
  datasets: [
    {
      label: 'Vendas',
      data: [40, 60, 80],
      backgroundColor: '#42A5F5'
    }
  ]
}

// Opções do gráfico
const chartOptions = {
  responsive: true,
  plugins: {
    legend: {
      position: 'top'
    }
  }
}
</script>

<template>
  <div class="w-96">
    <Bar :data="chartData" :options="chartOptions" />
  </div>
</template>

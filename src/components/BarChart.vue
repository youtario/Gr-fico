<script setup>
import { ref, reactive, computed } from 'vue'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'
import { Bar } from 'vue-chartjs'

// Registrar os módulos do Chart.js
ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

// Estados reativos para os controles
const chartType = ref('Gráfico')
const graphType = ref('Gráfico de colunas')
const timeRange = ref('30 dias')
const showMoreDonated = ref(true)
const showLessDonated = ref(true)

// Dados do gráfico (valores mais próximos da imagem)
const chartData = computed(() => ({
  labels: ['INSTITUIÇÃO ALMA', 'INSTITUIÇÃO NOVA'],
  datasets: [
    {
      data: [320, 150],
      backgroundColor: ['#4285F4', '#4285F4'],
      borderRadius: 0,
      barThickness: 60,
    }
  ]
}))

// Opções do gráfico para ficar mais próximo da imagem
const chartOptions = {
  responsive: true,
  maintainAspectRatio: false,
  layout: {
    padding: {
      top: 20,
      bottom: 40
    }
  },
  plugins: {
    legend: {
      display: false
    },
    tooltip: {
      enabled: true,
      backgroundColor: 'rgba(0,0,0,0.8)',
      titleColor: '#fff',
      bodyColor: '#fff'
    }
  },
  scales: {
    x: {
      display: true,
      grid: {
        display: false
      },
      ticks: {
        display: true,
        font: {
          size: 11
        },
        color: '#666',
        maxRotation: 0,
        minRotation: 0
      },
      border: {
        display: false
      }
    },
    y: {
      display: true,
      beginAtZero: true,
      max: 400,
      ticks: {
        stepSize: 50,
        font: {
          size: 11
        },
        color: '#999',
        callback: function(value) {
          return value
        }
      },
      grid: {
        display: true,
        color: '#f0f0f0',
        drawBorder: false
      },
      border: {
        display: false
      }
    }
  },
  elements: {
    bar: {
      borderWidth: 0
    }
  }
}
</script>

<template>
  <div class="donation-chart-container">
    <h2 class="chart-title">Registros de doação</h2>

    <div class="chart-wrapper">
      <!-- Área do Gráfico -->
      <div class="chart-area">
        <div class="chart-canvas">
          <Bar :data="chartData" :options="chartOptions" />
        </div>
      </div>

      <!-- Painel de Controles -->
      <div class="controls-panel">
        <!-- Tipo de registro -->
        <div class="control-group">
          <label class="control-label">Tipo de registro</label>
          <div class="select-wrapper">
            <select v-model="chartType" class="control-select">
              <option>Gráfico</option>
              <option>Texto</option>
            </select>
            <div class="select-arrow">▼</div>
          </div>
        </div>

        <!-- Tipo de gráfico -->
        <div class="control-group">
          <label class="control-label">Tipo de gráfico</label>
          <div class="radio-group">
            <label class="radio-item">
              <input
                v-model="graphType"
                type="radio"
                value="Gráfico de colunas"
                class="radio-input"
              />
              <span class="radio-label">Gráfico de colunas</span>
            </label>
            <label class="radio-item">
              <input
                v-model="graphType"
                type="radio"
                value="Gráfico pizza"
                class="radio-input"
              />
              <span class="radio-label">Gráfico pizza</span>
            </label>
          </div>
        </div>

        <!-- Conteúdo -->
        <div class="control-group">
          <label class="control-label">Conteúdo</label>
          <div class="checkbox-group">
            <label class="checkbox-item">
              <input
                v-model="showMoreDonated"
                type="checkbox"
                class="checkbox-input"
              />
              <span class="checkbox-label">Orgãos mais doados</span>
            </label>
            <label class="checkbox-item">
              <input
                v-model="showLessDonated"
                type="checkbox"
                class="checkbox-input"
              />
              <span class="checkbox-label">Filtros mais doados</span>
            </label>
          </div>
        </div>

        <!-- Faixa de tempo -->
        <div class="control-group">
          <label class="control-label">Faixa de tempo</label>
          <div class="select-wrapper">
            <select v-model="timeRange" class="control-select">
              <option>30 dias</option>
              <option>3 meses </option>
              <option>Ultimo ano</option>
              <option>Todos</option>
            </select>
            <div class="select-arrow">▼</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.donation-chart-container {
  background: #f8f9fa;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.chart-title {
  color: #2563EB;
  font-size: 18px;
  font-weight: 600;
  margin-bottom: 20px;
  margin-left: 0;
}

.chart-wrapper {
  display: flex;
  gap: 20px;
  background: white;
  border-radius: 6px;
  padding: 15px;
}

.chart-area {
  flex: 1;
  background: #fafafa;
  border-radius: 4px;
  padding: 15px;
  min-height: 350px;
}

.chart-canvas {
  height: 320px;
  width: 100%;
}

.controls-panel {
  width: 200px;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.control-group {
  display: flex;
  flex-direction: column;
}

.control-label {
  font-size: 12px;
  color: #666;
  margin-bottom: 8px;
  font-weight: 500;
}

.select-wrapper {
  position: relative;
}

.control-select {
  width: 100%;
  padding: 8px 30px 8px 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  background: white;
  font-size: 13px;
  appearance: none;
  cursor: pointer;
}

.select-arrow {
  position: absolute;
  right: 8px;
  top: 50%;
  transform: translateY(-50%);
  font-size: 10px;
  color: #999;
  pointer-events: none;
}

.radio-group,
.checkbox-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.radio-item,
.checkbox-item {
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
}

.radio-input,
.checkbox-input {
  margin: 0;
  accent-color: #2563EB;
}

.radio-label,
.checkbox-label {
  font-size: 13px;
  color: #333;
  cursor: pointer;
}

.control-select:focus {
  outline: none;
  border-color: #2563EB;
  box-shadow: 0 0 0 2px rgba(66, 133, 244, 0.1);
}

.radio-input:focus,
.checkbox-input:focus {
  outline: 2px solid #4285F4;
  outline-offset: 2px;
}
</style>

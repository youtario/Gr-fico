<script setup>
import { ref, reactive, computed } from 'vue'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'
import { Bar } from 'vue-chartjs'

// Registrar os módulos do Chart.js
ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

// Importar dados das ONGs
const ongs = [
  // CRIANÇAS
  { id: 'lar-abdon-batista', categoria: 'criancas', title: 'Lar Abdon Batista', doações: 320 },
  { id: 'omunga', categoria: 'criancas', title: 'OMUNGA', doações: 285 },
  { id: 'missao-crianca-jardim-paraiso', categoria: 'criancas', title: 'Missão Criança', doações: 240 },
  { id: 'ecos-esperanca', categoria: 'criancas', title: 'Ecos de Esperança', doações: 190 },
  { id: 'instituto-conforme', categoria: 'criancas', title: 'Instituto Conforme', doações: 165 },
  { id: 'casa-do-adalto', categoria: 'criancas', title: 'Casa do Adalto', doações: 150 },
  { id: 'projeto-resgate', categoria: 'criancas', title: 'Projeto Resgate', doações: 125 },
  { id: 'casa-lar-emanuel', categoria: 'criancas', title: 'Casa Lar Emanuel', doações: 110 },
  { id: 'instituto-caranguejo', categoria: 'criancas', title: 'Instituto Caranguejo', doações: 95 },

  // IDOSOS
  { id: 'lar-betania', categoria: 'idosos', title: 'Lar Betânia', doações: 275 },
  { id: 'ventura-residence', categoria: 'idosos', title: 'Ventura Residencial', doações: 220 },
  { id: 'lar-renascer', categoria: 'idosos', title: 'Lar Renascer', doações: 195 },
  { id: 'joao-de-paula', categoria: 'idosos', title: 'Exército da Salvação', doações: 180 },
  { id: 'lar-aconchego', categoria: 'idosos', title: 'Lar Aconchego', doações: 160 },
  { id: 'bom-retiro', categoria: 'idosos', title: 'Repouso Bom Retiro', doações: 140 },
  { id: 'casa-de-repouso-siloé', categoria: 'idosos', title: 'Casa SILOÉ', doações: 130 },
  { id: 'casa-repouso-acolher', categoria: 'idosos', title: 'Casa Acolher', doações: 115 },
  { id: 'acao-social-joinville', categoria: 'idosos', title: 'Ação Social JVE', doações: 105 },

  // MORADORES DE RUA
  { id: 'casa-santo-egidio', categoria: 'moradores-de-rua', title: 'Santo Egídio', doações: 250 },
  { id: 'casa-vo-joaquina', categoria: 'moradores-de-rua', title: 'Casa Vó Joaquina', doações: 210 },
  { id: 'crepsr', categoria: 'moradores-de-rua', title: 'Centro Pop', doações: 200 },
  { id: 'restaurante-herbert', categoria: 'moradores-de-rua', title: 'Rest. Popular 1', doações: 175 },
  { id: 'restaurante-zilda', categoria: 'moradores-de-rua', title: 'Rest. Popular 2', doações: 170 },
  { id: 'casa-de-levi', categoria: 'moradores-de-rua', title: 'Eis-me Aqui', doações: 145 },
  { id: 'sementes-do-futuro', categoria: 'moradores-de-rua', title: 'AACC Sementes', doações: 120 },
  { id: 'aaprn', categoria: 'moradores-de-rua', title: 'AACC Reabilitação', doações: 100 },
  { id: 'caps', categoria: 'moradores-de-rua', title: 'CAPS', doações: 85 }
]

// Estados reativos para os controles
const chartType = ref('Gráfico')
const graphType = ref('Gráfico de colunas')
const timeRange = ref('30 dias')
const showCriancas = ref(true)
const showIdosos = ref(true)
const showMoradoresRua = ref(true)

// Cores para cada categoria
const coresCategorias = {
  'criancas': '#4285F4',
  'idosos': '#34A853',
  'moradores-de-rua': '#EA4335'
}

// Dados computados baseados nos filtros
const chartData = computed(() => {
  const labels = []
  const data = []
  const cores = []

  ongs.forEach(ong => {
    let incluir = false

    if (ong.categoria === 'criancas' && showCriancas.value) incluir = true
    if (ong.categoria === 'idosos' && showIdosos.value) incluir = true
    if (ong.categoria === 'moradores-de-rua' && showMoradoresRua.value) incluir = true

    if (incluir) {
      labels.push(ong.title)
      data.push(ong.doações)
      cores.push(coresCategorias[ong.categoria])
    }
  })

  return {
    labels,
    datasets: [
      {
        label: 'Registros de Doação',
        data,
        backgroundColor: cores,
        borderRadius: 2,
        barThickness: 25,
      }
    ]
  }
})

// Estatísticas computadas
const estatisticas = computed(() => {
  const ongsVisiveis = ongs.filter(ong => {
    if (ong.categoria === 'criancas' && showCriancas.value) return true
    if (ong.categoria === 'idosos' && showIdosos.value) return true
    if (ong.categoria === 'moradores-de-rua' && showMoradoresRua.value) return true
    return false
  })

  const totalDoações = ongsVisiveis.reduce((total, ong) => total + ong.doações, 0)
  const totalOngs = ongsVisiveis.length

  return { totalDoações, totalOngs }
})

// Opções do gráfico
const chartOptions = {
  responsive: true,
  maintainAspectRatio: false,
  layout: {
    padding: {
      top: 20,
      bottom: 60
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
      bodyColor: '#fff',
      callbacks: {
        label: function(context) {
          return `${context.parsed.y} doações`
        }
      }
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
          size: 9
        },
        color: '#666',
        maxRotation: 45,
        minRotation: 45
      },
      border: {
        display: false
      }
    },
    y: {
      display: true,
      beginAtZero: true,
      max: 350,
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
          <label class="control-label">Categorias</label>
          <div class="checkbox-group">
            <label class="checkbox-item">
              <input
                v-model="showCriancas"
                type="checkbox"
                class="checkbox-input"
              />
              <span class="checkbox-label">
                <span class="category-dot criancas"></span>
                Crianças (9)
              </span>
            </label>
            <label class="checkbox-item">
              <input
                v-model="showIdosos"
                type="checkbox"
                class="checkbox-input"
              />
              <span class="checkbox-label">
                <span class="category-dot idosos"></span>
                Idosos (9)
              </span>
            </label>
            <label class="checkbox-item">
              <input
                v-model="showMoradoresRua"
                type="checkbox"
                class="checkbox-input"
              />
              <span class="checkbox-label">
                <span class="category-dot moradores-rua"></span>
                Moradores de Rua (9)
              </span>
            </label>
          </div>
        </div>

        <!-- Faixa de tempo -->
        <div class="control-group">
          <label class="control-label">Faixa de tempo</label>
          <div class="select-wrapper">
            <select v-model="timeRange" class="control-select">
              <option>30 dias</option>
              <option>3 meses</option>
              <option>Ultimo ano</option>
              <option>Todos</option>
            </select>
            <div class="select-arrow">▼</div>
          </div>
        </div>

        <!-- Estatísticas -->
        <div class="control-group">
          <div class="stats-box">
            <div class="stat-item">
              <div class="stat-number">{{ estatisticas.totalOngs }}</div>
              <div class="stat-label">ONGs Ativas</div>
            </div>
            <div class="stat-item">
              <div class="stat-number">{{ estatisticas.totalDoações.toLocaleString() }}</div>
              <div class="stat-label">Total Doações</div>
            </div>
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
  min-height: 400px;
}

.chart-canvas {
  height: 370px;
  width: 100%;
}

.controls-panel {
  width: 220px;
  display: flex;
  flex-direction: column;
  gap: 18px;
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
  display: flex;
  align-items: center;
  gap: 6px;
}

.category-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  display: inline-block;
}

.category-dot.criancas {
  background-color: #4285F4;
}

.category-dot.idosos {
  background-color: #34A853;
}

.category-dot.moradores-rua {
  background-color: #EA4335;
}

.stats-box {
  background: #f0f7ff;
  border: 1px solid #e3f2fd;
  border-radius: 6px;
  padding: 12px;
  display: flex;
  justify-content: space-between;
  gap: 10px;
}

.stat-item {
  text-align: center;
  flex: 1;
}

.stat-number {
  font-size: 18px;
  font-weight: bold;
  color: #2563EB;
  line-height: 1.2;
}

.stat-label {
  font-size: 10px;
  color: #666;
  margin-top: 2px;
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

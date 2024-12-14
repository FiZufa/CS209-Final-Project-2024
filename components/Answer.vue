<template>
    <h3>{{ title }}</h3>
    <div class="canvas-wrapper">
        <canvas ref="chartCanvas"></canvas>
    </div>
</template>
<script setup>
import { ref, watch, onMounted, onBeforeUnmount } from 'vue';
import { Chart, registerables } from 'chart.js';

Chart.register(...registerables);

const props = defineProps({
  chartLabels: {
    type: Array,
    required: true,
    default: () => []
  },
  chartData: {
    type: Array,
    required: true,
    default: () => []
  },
  title: {
    type: String,
    required: true,
    default: () => []
  }
});

const chartCanvas = ref(null);
let chartInstance = null;

// Utility function to generate random colors
const generateColors = (count) => {
  return Array.from({ length: count }, () =>
    `rgba(${Math.floor(Math.random() * 256)}, ${Math.floor(Math.random() * 256)}, ${Math.floor(Math.random() * 256)}, 0.7)`
  );
};


const initializeChart = () => {
  if (!chartCanvas.value || !props.chartLabels.length || !props.chartData.length) {
    console.warn('Cannot initialize chart: Missing canvas or props.');
    return;
  }

  if (chartInstance) {
    console.log('Destroying existing chart.');
    chartInstance.destroy();
  }

  chartInstance = new Chart(chartCanvas.value, {
    type: 'bar',
    data: {
      labels: props.chartLabels,
      datasets: [
        {
          label: '',
          data: props.chartData,
          backgroundColor: generateColors(props.chartData.length),
        },
      ],
    },
    options: {
      responsive: true,
      indexAxis: 'y',
      plugins: {
        legend: { display: true, position: 'top' },
      },
      scales: { x: { beginAtZero: true } },
    },
  });

  console.log('Chart initialized:', chartInstance);
};

watch(
  () => ({ labels: props.chartLabels, data: props.chartData }),
  ({ labels, data }) => {
    if (Array.isArray(labels) && labels.length && Array.isArray(data) && data.length) {
      initializeChart(); 
    }
  },
  { immediate: true, deep: true }
);

onMounted(() => {
  initializeChart();
});

onBeforeUnmount(() => {
  if (chartInstance) {
    chartInstance.destroy();
  }
});
</script>
<style scoped>
.canvas-wrapper {
  display: flex;
  justify-content: center; /* Center horizontally */
  align-items: center; /* Center vertically */
  height: 80vh; /* Adjust the height as needed */
  width:100vh;
}

canvas {
  height: 100% !important; /* Maintain aspect ratio */
  width: auto;
}
</style>
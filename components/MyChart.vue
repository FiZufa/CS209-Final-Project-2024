<template>
  <div class="canvas-wrapper">
    <canvas ref="chartCanvas"></canvas>
  </div>
</template>

<script setup>
import { ref, watch, onMounted, onBeforeUnmount } from 'vue';
import { Chart, registerables } from 'chart.js';

// Register all Chart.js components globally
Chart.register(...registerables);

const props = defineProps({
  chartLabels: {
    type: Array,
    required: true,
  },
  chartData: {
    type: Array,
    required: true,
  },
});

const chartCanvas = ref(null); // Reference to the canvas element
let chartInstance = null; // To store the Chart.js instance

const generateColors = (count) => {
  return Array.from({ length: count }, () =>
    `rgba(${Math.floor(Math.random() * 256)}, ${Math.floor(Math.random() * 256)}, ${Math.floor(Math.random() * 256)}, 0.7)`
  );
};

// Initialize colors
let backgroundColors = generateColors(props.chartData.length);

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
  () => ({ labels: props.chartLabels, data: props.chartData }), // Watch combined props
  ({ labels, data }) => {
    if (labels.length && data.length) {
      console.log('Props updated: Reinitializing chart.');
      initializeChart();
    }
  },
  { immediate: true, deep: true }
);

onMounted(() => {
  console.log('Initial chart data:', props.chartData);
  console.log('Initial chart labels:', props.chartLabels);
  initializeChart();

  if (chartInstance) {
    chartInstance.data.labels = props.chartLabels;
    chartInstance.data.datasets[0].data = props.chartData;
    chartInstance.data.datasets[0].backgroundColor = backgroundColors;
    chartInstance.update();
  }
});


onBeforeUnmount(() => {
  // Destroy the chart instance when the component is unmounted
  if (chartInstance) {
    chartInstance.destroy();
    chartInstance = null;
  }
});

</script>


<style scoped>
.canvas-wrapper {
  display: flex;
  justify-content: center; /* Center horizontally */
  align-items: center; /* Center vertically */
  height: 70vh; /* Adjust the height as needed */
}

canvas {
  width: 75% !important; /* Make the canvas fill the parent container */
  height: auto !important; /* Maintain aspect ratio */
}
</style>
  
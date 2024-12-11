<template>
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
  },
  chartData: {
    type: Array,
    required: true,
  },
});

const chartCanvas = ref(null);
let chartInstance = null;

// Utility function to generate random colors
const generateColors = (count) => {
  return Array.from({ length: count }, () =>
    `rgba(${Math.floor(Math.random() * 256)}, ${Math.floor(Math.random() * 256)}, ${Math.floor(Math.random() * 256)}, 0.7)`
  );
};

// Function to initialize the chart
const initializeChart = () => {
  if (!chartCanvas.value || !props.chartLabels.length || !props.chartData.length) {
    console.warn('Cannot initialize chart: Missing canvas or props.');
    return;
  }

  // Destroy existing chart instance if it exists
  if (chartInstance) {
    chartInstance.destroy();
  }

  // Create new chart instance
  chartInstance = new Chart(chartCanvas.value, {
    type: 'bar',
    data: {
      labels: props.chartLabels,
      datasets: [
        {
          label: '',
          data: props.chartData,
          backgroundColor: generateColors(props.chartData.length),
          borderRadius: 25,
          borderWidth: 4,
          borderSkipped: false,
        },
      ],
    },
    options: {
      responsive: true,
      plugins: {
        legend: { display: true, position: 'top' },
      },
      scales: {
        y: { beginAtZero: true }, // Vertical scale (Y-axis)
        x: { beginAtZero: false }, // Horizontal scale (X-axis)
      },
    },
  });
};

// Watch for changes in props and reinitialize the chart
watch(
  () => ({ labels: props.chartLabels, data: props.chartData }),
  ({ labels, data }) => {
    if (labels.length && data.length) {
      initializeChart(); // Reinitialize the chart if props change
    }
  },
  { immediate: true, deep: true }
);

// Initialize chart when component is mounted
onMounted(() => {
  initializeChart();
});

// Clean up the chart instance on unmount
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
  height: 70vh; /* Adjust the height as needed */
}

canvas {
  width: 75% !important; /* Make the canvas fill the parent container */
  height: auto !important; /* Maintain aspect ratio */
}
</style>
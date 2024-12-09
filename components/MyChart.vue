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

// Reactive variable for colors
let backgroundColors = generateColors(props.chartData.length);

// Watch for updates in props to re-render the chart
watch([props.chartLabels, props.chartData], () => {
  if (chartInstance) {
    // Update the chart when props change
    chartInstance.data.labels = props.chartLabels;
    chartInstance.data.datasets[0].data = props.chartData;
    backgroundColors = generateColors(props.chartData.length); // Generate new colors
    chartInstance.data.datasets[0].backgroundColor = backgroundColors;
    chartInstance.update();
    console.log(props.chartData)
    console.log(props.chartLabels)
  }
});

onMounted(() => {
  // Initialize the chart when the component is mounted
  chartInstance = new Chart(chartCanvas.value, {
    type: 'bar', // Choose the chart type
    data: {
      labels: props.chartLabels,
      datasets: [
        {
          label: 'Total Engagement',
          data: props.chartData,
          backgroundColor: backgroundColors, // Use generated colors
        },
      ],
    },
    options: {
      responsive: true,
      indexAxis: 'y', // For horizontal bar chart
      plugins: {
        legend: {
          display: true,
          position: 'top',
        },
      },
      scales: {
        x: {
          beginAtZero: true,
        },
      },
    },
  });
});

onBeforeUnmount(() => {
  // Destroy the chart instance when the component is unmounted
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
  
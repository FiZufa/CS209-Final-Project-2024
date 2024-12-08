<template>
    <div class="canvas-wrapper">
      <canvas ref="chartCanvas"></canvas>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted, onBeforeUnmount } from 'vue';
  import { Chart, registerables } from 'chart.js';
  
  // Register all Chart.js components globally
  Chart.register(...registerables);
  
  const chartCanvas = ref(null); // Reference to the canvas element
  let chartInstance = null; // To store the Chart.js instance
  
  onMounted(() => {
    // Initialize the chart when the component is mounted
    chartInstance = new Chart(chartCanvas.value, {
      type: 'bar', // Change this to 'line', 'pie', etc. for other chart types
      data: {
        labels: ['January', 'February', 'March', 'April'], // Example labels
        datasets: [
          {
            label: '',
            data: [10, 20, 30, 40], // Example data
            backgroundColor: ['#FF6384', '#36A2EB', '#FFCE56', '#4BC0C0'],
          },
        ],
      },
      options: {
        responsive: true,
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
  
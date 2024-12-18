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

const initializeChart = () => {
  let correlationText = '';
  
  if (!chartCanvas.value || !props.chartLabels.length || !props.chartData.length) {
    console.warn('Cannot initialize chart: Missing canvas or props.');
    return;
  }

  console.log(props.chartData.length);

  if (chartInstance) {
    console.log('Destroying existing chart.');
    chartInstance.destroy();
  }

  // Combine chartLabels (x-axis) and chartData (y-axis) into scatter plot points
  const scatterData = props.chartLabels.map((x, index) => ({
    x, // x comes from chartLabels
    y: props.chartData[index], // y comes from chartData
  }));

  // Step 1: Calculate the trendline
  const calculateTrendline = (data) => {
    const n = data.length;
    const sumX = data.reduce((sum, point) => sum + point.x, 0);
    const sumY = data.reduce((sum, point) => sum + point.y, 0);
    const sumXY = data.reduce((sum, point) => sum + point.x * point.y, 0);
    const sumX2 = data.reduce((sum, point) => sum + point.x * point.x, 0);

    const slope = (n * sumXY - sumX * sumY) / (n * sumX2 - sumX * sumX);
    const intercept = (sumY - slope * sumX) / n;

    return { slope, intercept };
  };

  const calculateCorrelation = (x, y) => {
  const n = x.length;
  const sumX = x.reduce((sum, val) => sum + val, 0);
  const sumY = y.reduce((sum, val) => sum + val, 0);
  const sumXY = x.reduce((sum, val, i) => sum + val * y[i], 0);
  const sumX2 = x.reduce((sum, val) => sum + val * val, 0);
  const sumY2 = y.reduce((sum, val) => sum + val * val, 0);

  const numerator = n * sumXY - sumX * sumY;
  const denominator = Math.sqrt(
    (n * sumX2 - sumX * sumX) * (n * sumY2 - sumY * sumY)
  );

    return denominator === 0 ? 0 : numerator / denominator;
  };

  const { slope, intercept } = calculateTrendline(scatterData);

  // Generate the trendline points
  const trendlineData = props.chartLabels.map(x => ({
    x,
    y: slope * x + intercept, // y = mx + b
  }));

  const xValues = props.chartLabels;
  const yValues = props.chartData;

  if (xValues.length === yValues.length) {
    const correlation = calculateCorrelation(xValues, yValues);
    correlationText = `Correlation: r = ${correlation.toFixed(2)}`;
    console.log('Correlation Coefficient (r):', correlation);
  } else {
    console.warn('x and y arrays must have the same length.');
  }

  // Step 2: Add both scatter and trendline datasets to the chart
  chartInstance = new Chart(chartCanvas.value, {
    type: 'scatter',
    data: {
      datasets: [
        {
          label: props.title || 'Scatter Plot', // Title for the scatter plot
          data: scatterData, // Scatter data
          backgroundColor: 'rgba(75, 192, 192, 0.6)', // Custom color
        },
        {
          label: 'Trendline', // Label for trendline
          data: trendlineData, // Trendline data
          type: 'line', // Render as a line
          borderColor: 'rgba(255, 99, 132, 0.8)', // Line color
          borderWidth: 2,
          fill: false,
        },
      ],
    },
    options: {
      responsive: true,
      plugins: {
        legend: { display: true, position: 'top' },
        title: {
          display: true,
          text: `${props.title || 'Scatter Plot'} (${correlationText})`, // Include correlation in the title
        },
      },
      scales: {
        x: {
          beginAtZero: true,
          title: { display: true, text: props.title }, // Label for x-axis
        },
        y: {
          beginAtZero: true,
          title: { display: true, text: 'number of upvotes' }, // Label for y-axis
        },
      },
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
<template>
    <AppHeader />

    <h1>Most Frequently Asked Topics</h1>

    <div class="chart-container">
        <MyChart />
    </div>

    <div class="input-container">
        <label for="dataNumber">Enter the number of data points:</label>
        <input
          v-model="dataNumber"
          id="dataNumber"
          type="number"
          min="1"
          placeholder="Enter a number"
        />
        <button @click="analyzeData">Analyse</button>
      </div>
    
</template>
  
<script>
import AppHeader from '@/components/AppHeader.vue';
import MyChart from '@/components/MyChart.vue';

export default {
    name: 'JavaTopic'
};

const chartType = ref('bar'); // Default chart type
const chartData = ref([
  {
    label: 'Sample Data',
    data: [10, 20, 30, 40],
    backgroundColor: ['#FF6384', '#36A2EB', '#FFCE56', '#4BC0C0'],
  },
]);

const analyzeData = () => {
  if (dataNumber.value && dataNumber.value > 0) {
    alert(`Analyzing data for ${dataNumber.value} points.`);
    // You can update `chartData` dynamically based on user input here
  } else {
    alert('Please enter a valid number greater than 0');
  }
};

</script>
  
<style scoped>
/* Reset global centering to avoid conflicts */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

/* Center the chart container */
.chart-container {
    display: flex; /* Use Flexbox */
    justify-content: center; /* Center horizontally */
    align-items: center; /* Center vertically within the container */
    height: 70vh; /* Adjust height to fit the viewport as needed */
    margin: 0 auto; /* Optional: Centers the container itself */
}

/* Center the header text */
h1 {
    text-align: center;
    margin-bottom: 20px; /* Add spacing below the header */
    margin-top: 30px;
    margin-bottom: 20px;
}

.input-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 20px;
    margin-bottom: 50px;
  }
  
  .input-container input {
    padding: 10px;
    margin: 10px;
    width: 200px;
    text-align: center;
  }
  
  .input-container button {
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
    background-color: #4CAF50;
    color: white;
    border: none;
    border-radius: 5px;
  }
  
  .input-container button:hover {
    background-color: #45a049;
  }
</style>
  
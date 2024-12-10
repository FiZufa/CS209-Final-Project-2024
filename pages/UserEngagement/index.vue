<template>
    <AppHeader />

    <h1>Most Engaged Topics</h1>

    <!-- Loading Screen -->
    <div v-if="loading" class="loading-screen">
      <div class="spinner"></div>
    </div>

    <div class="chart-container">
        <MyEngChart :chartLabels="chartLabels" :chartData="chartData" />
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
import MyEngChart from '@/components/MyEngChart.vue';
import axios from 'axios';

export default {
  name: 'UserEngagement',
  components: {
    AppHeader,
    MyEngChart,
  },
  data() {
    return {
      chartLabels: [], // Labels for the chart
      chartData: [],   // Data points for the chart
      dataNumber: null,  
      loading: false,
    };
  },
  mounted() {
    
    this.fetchTopTags();
  },
  methods: {
    async fetchTopTags() {
      this.loading = true;
      try {
        
        const response = await axios.get(`http://35.240.167.146:16800/api/v1/questions/top-engagement-tags/${this.dataNumber || 10}`);
        const tags = response.data;

        if (tags && tags.length) {
          console.log('Fetched tags:', tags);
          // Map the JSON data to labels and data arrays
          this.chartLabels = tags.map(tag => tag.name);
          this.chartData = tags.map(tag => tag.totalEngagement);
        } else {
          console.warn('No tags returned from API.');
          this.chartLabels = [];
          this.chartData = [];
        }
      } catch (error) {
        console.error('Error fetching top tags:', error);
        this.chartLabels = [];
        this.chartData = [];
      } finally {
        this.loading = false;
      }
    },

    analyzeData() {
      if (this.dataNumber && this.dataNumber > 0) {
        // Fetch tags based on the number input by the user
        this.fetchTopTags();
      } else {
        alert('Please enter a valid number greater than 0');
      }
    },
  },
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

  .loading-screen {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.5);
    color: white;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 20px;
  }
  
  .spinner {
    border: 4px solid #f3f3f3;
    border-top: 4px solid #3498db;
    border-radius: 50%;
    width: 50px;
    height: 50px;
    animation: spin 2s linear infinite;
  }
  
  @keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }
</style>
  
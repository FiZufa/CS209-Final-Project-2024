<template>
    <AppHeader />

    <h1>Answer Quality Analysis</h1>


        <!-- Loading Screen -->
        <div v-if="loading" class="loading-screen">
          <div class="spinner"></div>
        </div>

    <div class="body">
      <div class="btn-option">
        <button class="original-data-btn">Original data</button>
        <button class="scaled-data-btn">Scaled data</button>
      </div>

      <div class="charts">
      
        <div class="chart-container">
            <Answer :chartData="normalizeScore" :chartLabels="normalizeReputation" :title="'Owner Reputation'" />
        </div>
    
        <div class="chart-container">
            <Answer :chartData="normalizeScore" :chartLabels="normalizeTime" :title="'Time Elapsed'" />
        </div>
        
        <div class="chart-container">
          <Answer :chartData="normalizeScore" :chartLabels="normalizeLength" :title="'Answer Length'" />
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

    </div>

  </div>

</template>
  
<script>
import AppHeader from '@/components/AppHeader.vue';
import Answer from '@/components/Answer.vue';
import axios from 'axios';

export default {
  name: 'AnswerQuality',
  components: {
    AppHeader,
    Answer,
  },
  data() {
    return {
      isAccepted: [],
      timeElapsed: [],
      ownerReputation: [],
      upvote: [],
      answerLength: [],
      normalizeTime: [],
      normalizeReputation: [],
      normalizeLength: [],
      normalizeScore: [],
      dataNumber: null,
      loading: false,

    };
  },
  mounted() {
    this.fetchAnswerData(); 
  },
  methods: {
  async fetchAnswerData() {
    this.loading = true;
    try {
      const response = await axios.get(`http://35.240.167.146:16800/api/v1/questions/overall-answer-quality/${this.dataNumber || 1000}`);
      const tags = response.data;

      if (tags) {
        this.isAccepted = tags.map(tag => tag.isAccepted);
        this.timeElapsed = tags.map(tag => tag.timeElapse/60);
        this.ownerReputation = tags.map(tag => tag.ownerReputation);
        this.upvote = tags.map(tag => tag.score);
        this.answerLength = tags.map(tag => tag.answerLength);

        this.normalizeData();
      } else {
        console.warn('No tags returned from API.');
        this.resetData();
      }
    } catch (error) {
      console.error('Error fetching top tags:', error);
      this.resetData();
    } finally {
      this.loading = false;
    }
  },

  normalizeData() {
    const logTransform = (array) => array.map(value => Math.log(value + 1)); // Add 1 to avoid log(0)

    // Example in normalizeData
    const normalize = (array) => {
    const transformed = logTransform(array); // Apply log transformation
    const min = Math.min(...transformed);
    const max = Math.max(...transformed);

    if (min === max) {
      return transformed.map(() => 0.5);
    }

    return transformed.map(value => {
      const normalized = (value - min) / (max - min);
      return Math.min(normalized, 1.0); // Clamp if needed
    });
  };


  this.normalizeTime = normalize(this.timeElapsed);
  this.normalizeReputation = normalize(this.ownerReputation);
  this.normalizeLength = normalize(this.answerLength);
  this.normalizeScore = normalize(this.upvote);

  console.log('Normalized Time:', this.normalizeTime);
  console.log('Normalized Reputation:', this.normalizeReputation);
  console.log('Normalized Length:', this.normalizeLength);
  console.log('Normalized Score:', this.normalizeScore);
},
  resetData() {
    this.isAccepted = [];
    this.timeElapsed = [];
    this.ownerReputation = [];
    this.upvote = [];
    this.answerLength = [];
    this.normalizeTime = [];
    this.normalizeReputation = [];
    this.normalizeLength = [];
  },
  analyzeData() {
      if (this.dataNumber && this.dataNumber > 0) {
        // Fetch tags based on the number input by the user
        this.fetchAnswerData();
      } else {
        alert('Please enter a valid number greater than 0');
      }
    },
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

.body {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.charts {
  display: flex;
  flex-direction: column;
}

.chart-container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center; 
    margin: 0 auto; 
}

h1 {
    text-align: center;
    margin-bottom: 20px; 
    margin-top: 30px;
    margin-bottom: 20px;
}

.input-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-bottom: 20px;
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

  .btn-option {
    display: flex;
    justify-content: center; /* Center horizontally */
    align-items: center; /* Align vertically */
    gap: 20px; /* Space between buttons */
    margin: 20px auto; /* Add spacing */
    padding: 10px;
  }
  
  /* Style for the buttons */
  .btn-option button {
    background-color: #4caf50; /* Green background */
    color: white; /* White text */
    border: none;
    padding: 10px 20px;
    font-size: 1em;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  .btn-option button:hover {
    background-color: #388e3c; /* Darker green on hover */
  }
  
  .btn-option button:active {
    background-color: #2e7d32; /* Even darker green when clicked */
  }


</style>
  
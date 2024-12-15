<template>
    <AppHeader />

    <h1>Answer Quality Analysis</h1>

        <!-- Loading Screen -->
        <div v-if="loading" class="loading-screen">
          <div class="spinner"></div>
        </div>

    
    <div class="charts">
        <div class="chart-container">
            <Answer :chartData="avgScore" :chartLabels="isAccepted" :title="'Answer Score'"/>
          </div>
      
          <div class="chart-container">
              <Answer :chartData="avgOwnerReputation" :chartLabels="isAccepted" :title="'Owner Reputation'"/>
          </div>
      
          <div class="chart-container">
              <Answer :chartData="avgTimeElapse" :chartLabels="isAccepted" :title="'Time Elapsed'"/>
          </div>
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
      avgTimeElapse: [],
      avgOwnerReputation: [],
      avgScore: [],
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
        // Fetch the top tags based on the user input (dataNumber)
        const response = await axios.get(`http://35.240.167.146:16800/api/v1/questions/overall-answer-quality/${this.dataNumber || 1000}`);
        const tags = response.data;

        if (tags) {
          console.log('Fetched tags:', tags);
          const isAcceptedKeys = Object.keys(tags); // ["false", "true"]
          this.isAccepted = isAcceptedKeys.map(key => tags[key].isAccepted);
          this.avgTimeElapse = isAcceptedKeys.map(key => tags[key].avgTimeElapse);
          this.avgOwnerReputation = isAcceptedKeys.map(key => tags[key].avgOwnerReputation);
          this.avgScore = isAcceptedKeys.map(key => tags[key].avgScore);

        } else {
          console.warn('No tags returned from API.');
          this.resetData()
        }
      } catch (error) {
        console.error('Error fetching top tags:', error);
        this.resetData()
      } finally {
        this.loading = false;
      }
    },

    analyzeData() {
      if (this.dataNumber && this.dataNumber > 0) {
        // Fetch tags based on the number input by the user
        this.fetchAnswerData();
      } else {
        alert('Please enter a valid number greater than 0');
      }
    },

    resetData() {
      this.isAccepted = [];
      this.avgTimeElapse = [];
      this.avgOwnerReputation = [];
      this.avgScore = [];
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

.charts {
  display: flex;
  flex-direction: row;
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
</style>
  
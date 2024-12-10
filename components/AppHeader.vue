<template>
    <header class="header">
        <div class="logo">
            <img  src="@/assets/stacklogo.png" alt="logo" @click="gotoAnotherPage('/')">
        </div>
        <div class="menus">
            <div class="topic-container">
                <img src="@/assets/java.png" alt="image" class="img">
                <button class="java-btn" @click="gotoAnotherPage('/JavaTopic')">Topic frequency</button>
            </div>

            <div class="user-container">
                <img src="@/assets/user.png" alt="image" class="img">
                <button class="user-btn" @click="gotoAnotherPage('/UserEngagement')">User engagement</button>
            </div>

            <div class="error-container">
                <img src="@/assets/error.png" alt="image" class="img">
                <button class="error-btn" @click="gotoAnotherPage('/Errors')">Errors</button>
            </div>

            <div class="answer-container">
                <img src="@/assets/answer.png" alt="image" class="img">
                <button class="answer-btn">Answer quality</button>
            </div>

            <div class="search-container">
              <img src="@/assets/search.png" alt="image" class="img-search" @click="togglePopup">
            </div>
        </div>

        <!-- Popup Box -->
    <div v-if="showPopup" class="popup-overlay">
      <div class="popup-box">
        <h2>Search Box</h2>
        <input type="text" v-model="searchTerm" placeholder="Enter Java Topic or Error keyword..." />
        <div class="btn-container">
          <button class="search-topic-btn" @click="searchJavaTopic">Search Java Topic</button>
          <button class="search-error-btn">Search Error or Exception</button>
        </div>

        <!-- Results Section -->
        <div v-if="javaTopicsearchResults" class="results-box">
          <p>Frequency for "<span class="span-term">{{ searchTerm }}</span>" is <span class="span-result">{{ javaTopicsearchResults }}</span></p>
        </div>

        <button class="close-btn" @click="closePopup">close</button>
      </div>
    </div>

    </header>

</template>
<script setup>
import { ref } from 'vue';  // Import ref from Vue for reactive data
import { useRouter } from 'vue-router';  // Import the useRouter hook
import axios from 'axios';

const showPopup = ref(false); 
const searchTerm = ref('');
const javaTopicsearchResults = ref('');

const router = useRouter();

const gotoAnotherPage = (path) => {
  console.log('Navigating to:', path); 
  router.push(path);
};

const togglePopup = () => {
  showPopup.value = !showPopup.value; 
};

const closePopup = () => {
  showPopup.value = false; 
  searchTerm.value = ''; // Clear the input field
  javaTopicsearchResults.value = ''; // Clear the search results
};

const searchJavaTopic = async () => {
  if (!searchTerm.value.trim()) {
    alert('Please enter a Java topic!');
    return;
  }

  const endpoint = `http://35.240.167.146:16800/api/v1/questions/frequency/${searchTerm.value}`;
  try {
    const response = await axios.get(endpoint);
    console.log('Search Java Topic Response:', response.data);

    javaTopicsearchResults.value = response.data;

    //alert(`Frequency for "${searchTerm.value}": ${response.data}`);
  } catch (error) {
    console.error('Error fetching Java topic data:', error);
    alert('An error occurred while fetching data. Please try again.');
  }
};

</script>
<style scoped>

html, body {
    margin: 0;
    padding: 0;
    height: 100%;
}

.header {
    margin: 0;
    padding: 0;
    top: 0;
    position: sticky;
    color: #fff;
    background-color: #fff;
    
    text-align: center;
    align-items: center;
    display: flex;
    flex-direction: row;
    margin-top: 0;
    border-bottom: 1px solid #ccc;

  }

  .logo{
    margin-left: 6em;
    float: left;
    margin-right: 6em;
  }

  .logo img{
    height: 65px;
    cursor: pointer;
    margin-right: 10px;
  }

  .right{
    display: flex;
    float: right;
  }
  
  .menus {
    display: flex;
    justify-content: space-around;
    align-items: center;
    flex-wrap: wrap;
  }
  
  .menu-container {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  
  .img {
    width: 35px;
    height: 35px;
    margin-bottom: 10px;
  }
  
  button {
    display: flex;
    flex-direction: row;
    align-items: center;
    padding: 10px;
    background: none;
    border: none;
    cursor: pointer;
    font-size: 1em;
    gap: 0.1em;
    opacity: 70%;
  }
  
  button:hover {
    opacity: 100%;
  }

  .menus .img {
    align-items: center;
    text-align: center;
    justify-content: center;

    width: 2.0em;
    height: 2.0em;
    opacity: 70%;
  }

  .img-search {
    width: 2.5em;
    height: 2.5em;

    cursor: pointer;
    opacity: 0.7;
  }

  .img-search:hover {
    opacity: 1.0;
  }

  .topic-container,
  .user-container,
  .error-container,
  .answer-container {
    display: flex;
    flex-direction: row;  /* Stack images and buttons vertically */
    align-items: center;     /* Center the contents horizontally */
    margin: 0 10px; 
  }

  .search-container {
    position: relative;
    display: inline-block;
  }
  
  .img-search {
    cursor: pointer;
    width: 50px; /* Set image size as needed */
    height: 50px; /* Set image size as needed */
  }
  
  .popup-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5); /* Semi-transparent background */
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .popup-box h2 {
    font-size: 24px;
    margin-bottom: 20px;
    margin-top: 40px;
    color: #000;
  }
  
  .popup-box {
    background: white;
    padding: 45px;
    border-radius: 5px;
    width: 700px;
    height: 300px;
    margin-top: 10px;
    display: flex; /* Ensure popup content is displayed as flex */
    justify-content: center;
    flex-direction: column; /* Stack content vertically */
    align-items: center; /* Center the content horizontally */
  }
  
  .popup-box input {
    width: 60%;
    padding: 8px;
    margin-bottom: 10px;
  }
  
  .btn-container {
    margin: 5px;
    display: flex;
    flex-direction: row;
  }

  .search-topic-btn, .search-error-btn {
    padding: 8px;
    background-color: darkgreen;
    color: #fff;
    border: none;
    cursor: pointer;
    border-radius: 5px;
    margin: 0 10px;
  }
  .search-topic-btn:hover, .search-error-btn:hover {
    background-color: rgb(6, 58, 6);
    cursor: pointer;
  }

  .close-btn {
    background-color: #ccc;
    padding: 8px;
    border-radius: 5px;
    margin-top: 10px;
  }

  .close-btn:hover {
    background-color: darkgrey;
  }

  .results-box p {
    color: black;
  }

  .span-result {
    color: red;
    font-size: 20px;
  }

  .span-term {
    color: blue;
  }
</style>
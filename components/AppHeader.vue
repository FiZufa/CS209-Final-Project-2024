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
        </div>
    </header>

</template>
<script setup>
import { useRouter } from 'vue-router';  // Import the useRouter hook

const router = useRouter();  // Create a router instance

// Define the gotoAnotherPage function
const gotoAnotherPage = (path) => {
  console.log('Navigating to:', path);  // Debugging line
  router.push(path);
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
    width: 25px;
    height: 25px;
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

  .topic-container,
  .user-container,
  .error-container,
  .answer-container {
    display: flex;
    flex-direction: row;  /* Stack images and buttons vertically */
    align-items: center;     /* Center the contents horizontally */
    margin: 0 10px; 
  }
</style>
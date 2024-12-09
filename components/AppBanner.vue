<template>
    <div class="banner">
        <div class="headline">
            <h1>Unlock Insights from <span class="stack">Stack</span><span class="overflow">overflow</span> Data</h1>
        </div>

        <div class="menus">
            <button @click="gotoAnotherPage('/JavaTopic')">Topic frequency</button>
            <button @click="gotoAnotherPage('/UserEngagement')">User engagement</button>
            <button @click="gotoAnotherPage('/Errors')">Error Analysis</button>
            <button>Answer quality</button>
        </div>

        <!-- <div class="menus">
            <router-link to="/topic-frequency">
              <button>Topic frequency</button>
            </router-link>
            <router-link to="/user-engagement">
              <button>User engagement</button>
            </router-link>
            <router-link to="/error-analysis">
              <button>Error Analysis</button>
            </router-link>
            <router-link to="/answer-quality">
              <button>Answer quality</button>
            </router-link>
        </div> -->
    </div>
</template>
<script setup>
import { useRouter } from 'vue-router';  // Import the useRouter hook

const router = useRouter();  // Create a router instance

// Define the gotoAnotherPage function
const gotoAnotherPage = (path) => {
  console.log('Navigating to:', path);  // Debugging line
  router.push(path);
};


//defineExpose({ gotoAnotherPage });

</script>
<style scoped>

.banner {
    text-align: center;
    display: flex;                /* Use Flexbox for layout */
    flex-direction: column;       /* Stack child elements (headline and menus) vertically */
    justify-content: center;      /* Vertically center the items */
    align-items: center;          /* Horizontally center the items */
    height: 90vh;                /* Full viewport height */
    width: 100%;                  /* Full viewport width */
    margin: 0;                    /* Remove any margin */
    padding: 0;                   /* Remove any padding */
    background-image: url('@/assets/bg.jpg');
    background-size: cover;      /* Ensure the background image covers the entire area */
    background-position: center; /* Center the background image */
    background-repeat: no-repeat; /* Prevent background repeat */
}

.headline h1 {
    color: white;                /* Make text white to be visible on background */
    font-size: 2.5em;            /* Adjust the font size */
    margin: 0;                   /* Remove margin around the heading */
    padding: 1em;                /* Add some space around the heading */
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);  /* Optional: add shadow for visibility */
}

.menus {
    display: flex;               /* Use Flexbox for button layout */
    flex-direction: row;      /* Stack buttons vertically */
    align-items: center;         /* Center buttons horizontally */
    gap: 1em;                    /* Space between buttons */
    margin-top: 20px;             /* Add margin between heading and menus */
}

button {
    padding: 10px 20px;
    font-size: 1.2em;
    background-color: rgba(255, 255, 255, 0.7);  /* Slightly transparent background */
    border: none;
    cursor: pointer;
    transition: background-color 0.3s;
    border-radius: 5px;          /* Optional: round button corners */
}

button:hover {
    background-color: rgba(255, 255, 255, 1);    /* Solid background on hover */
}

.stack {
  font-weight: normal;
  color: #2C2D2D;
  text-shadow: 2px 2px 4px rgba(204, 204, 204, 0.5);
}

.overflow {
  font-weight: bold;
  color: #2C2D2D;
  text-shadow: 2px 2px 4px rgba(204, 204, 204, 0.5);
}



</style>
<template>
    <footer>
      <div class="footer-container">
        <div class="footer-column" v-for="(column, index) in footerColumns" :key="index">
          <h3>{{ column.title }}</h3>
          <ul class="footer-links">
            <li v-for="(link, linkIndex) in column.links" :key="linkIndex">
              <a v-if="link.title && link.url" :href="link.url">{{ link.title }}</a>
            </li>
          </ul>
        </div>
      </div>
    </footer>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  import { fetchEntries } from '@builder.io/sdk-vue';
  
  // Initialize Builder with your API key
  const apiKey = '6641374915bc47c1b0bcff83bbe6e797';
  
  const footerColumns = ref([]);
  
  // Fetch all footer links
  onMounted(async () => {
    try {
      const response = await fetchEntries({
        model: 'footer-links',
        apiKey,
      });
      footerColumns.value = response.map(entry => entry.data);
    } catch (error) {
      console.error('Error fetching footer links:', error);
    }
  });
  </script>
  
  <style scoped>
  /* Add your custom styles here */
  footer {
    background-color: #333;
    padding: 20px 0;
    color: #fff;
  }
  
  .footer-container {
    display: flex;
    justify-content: space-around;
    flex-wrap: wrap;
  }
  
  .footer-column {
    flex: 1;
    margin: 10px;
    min-width: 200px;
  }
  
  .footer-column h3 {
    margin-bottom: 10px;
  }
  
  .footer-links {
    list-style: none;
    padding: 0;
    margin: 0;
  }
  
  .footer-links li {
    margin-bottom: 10px;
  }
  
  .footer-links a {
    color: #fff;
    text-decoration: none;
    transition: color 0.3s;
  }
  
  .footer-links a:hover {
    color: #ccc;
  }
  </style>
  
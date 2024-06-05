<template>
    <header class="mega-header">
      <div class="brand">
        <img v-if="logo" :src="logo" alt="Brand Logo" class="brand-logo" />
        <span v-if="brandName" class="brand-name">{{ brandName }}</span>
      </div>
      <nav>
        <ul class="nav-menu">
          <li v-for="(linkEntry, index) in linkEntries" :key="index" class="nav-item">
            <a v-if="linkEntry.title && linkEntry.url" :href="linkEntry.url">{{ linkEntry.title }}</a>
            <div v-if="linkEntry.subLinks && linkEntry.subLinks.length" class="mega-menu">
              <div v-for="(subLink, subIndex) in linkEntry.subLinks" :key="subIndex" class="mega-menu-column">
                <h3>{{ subLink.title }}</h3>
                <ul>
                  <li v-for="(subSubLink, subSubIndex) in subLink.subLinks" :key="subSubIndex">
                    <a v-if="subSubLink.title && subSubLink.url" :href="subSubLink.url">{{ subSubLink.title }}</a>
                  </li>
                </ul>
              </div>
            </div>
          </li>
        </ul>
      </nav>
    </header>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  import { fetchEntries } from '@builder.io/sdk-vue';
  import { defineProps } from 'vue';
  
  // Define the props for the brand name and logo
  const props = defineProps({
    brandName: {
      type: String,
      required: false
    },
    logo: {
      type: String,
      required: false
    }
  });
  
  // Initialize Builder with your API key
  const apiKey = '6641374915bc47c1b0bcff83bbe6e797';
  
  const linkEntries = ref([]);
  
  // Fetch all navigation links
  onMounted(async () => {
    try {
      const response = await fetchEntries({
        model: 'nav-links',
        apiKey,
      });
      linkEntries.value = response.map(entry => entry.data);
    } catch (error) {
      console.error('Error fetching navigation links:', error);
    }
  });
  </script>
  
  <style scoped>
  /* Add your custom styles here */
  header {
    background-color: #333;
    padding: 10px 0;
  }
  
  .brand {
    display: flex;
    align-items: center;
    margin-bottom: 10px;
  }
  
  .brand-logo {
    height: 40px;
    margin-right: 10px;
  }
  
  .brand-name {
    color: #fff;
    font-size: 1.5em;
  }
  
  nav {
    display: flex;
    justify-content: center;
  }
  
  .nav-menu {
    list-style: none;
    padding: 0;
    margin: 0;
    display: flex;
    gap: 20px;
  }
  
  .nav-item {
    position: relative;
  }
  
  .nav-item > a {
    color: #fff;
    text-decoration: none;
    padding: 10px 15px;
    display: block;
    transition: background-color 0.3s, color 0.3s;
  }
  
  .nav-item > a:hover {
    background-color: #575757;
    color: #fff;
  }
  
  .mega-menu {
    display: none;
    position: absolute;
    left: 0;
    top: 100%;
    background-color: #444;
    padding: 20px;
    margin: 0;
    min-width: 600px;
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
    z-index: 1000;
    flex-wrap: wrap;
    gap: 20px;
  }
  
  .mega-menu-column {
    flex: 1;
    min-width: 200px;
  }
  
  .mega-menu-column h3 {
    color: #fff;
    font-size: 1.2em;
    margin-bottom: 10px;
  }
  
  .mega-menu-column ul {
    list-style: none;
    padding: 0;
    margin: 0;
  }
  
  .mega-menu-column li {
    margin-bottom: 5px;
  }
  
  .mega-menu-column li a {
    color: #fff;
    text-decoration: none;
    display: block;
    padding: 5px 0;
    transition: background-color 0.3s, color 0.3s;
  }
  
  .mega-menu-column li a:hover {
    background-color: #575757;
  }
  
  .nav-item:hover > .mega-menu {
    display: flex;
  }
  
  @media (max-width: 768px) {
    .nav-menu {
      flex-direction: column;
      gap: 0;
    }
  
    .nav-item {
      width: 100%;
    }
  
    .nav-item > a {
      padding: 10px;
    }
  
    .mega-menu {
      position: static;
      display: none;
      flex-direction: column;
    }
  
    .nav-item:hover > .mega-menu {
      display: flex;
    }
  }
  </style>
  
<template>
    <header>
      <div class="brand">
        <img v-if="logo" :src="logo" alt="Brand Logo" class="brand-logo" />
        <span v-if="brandName" class="brand-name">{{ brandName }}</span>
      </div>
      <nav>
        <ul class="nav-menu">
          <li v-for="(linkEntry, index) in linkEntries" :key="index" class="nav-item">
            <a v-if="linkEntry.title && linkEntry.url" :href="linkEntry.url">{{ linkEntry.title }}</a>
            <ul v-if="linkEntry.subLinks && linkEntry.subLinks.length" class="submenu">
              <li v-for="(subLink, subIndex) in linkEntry.subLinks" :key="subIndex" class="submenu-item">
                <a v-if="subLink.title && subLink.url" :href="subLink.url">{{ subLink.title }}</a>
                <ul v-if="subLink.subLinks && subLink.subLinks.length" class="subsubmenu">
                  <li v-for="(subSubLink, subSubIndex) in subLink.subLinks" :key="subSubIndex" class="subsubmenu-item">
                    <a v-if="subSubLink.title && subSubLink.url" :href="subSubLink.url">{{ subSubLink.title }}</a>
                  </li>
                </ul>
              </li>
            </ul>
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
  
  .submenu,
  .subsubmenu {
    display: none;
    position: absolute;
    left: 0;
    top: 100%;
    background-color: #444;
    list-style: none;
    padding: 10px 0;
    margin: 0;
    min-width: 200px;
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
    z-index: 1000;
  }
  
  .submenu .submenu-item {
    position: relative;
  }
  
  .subsubmenu {
    top: 0;
    left: 100%;
  }
  
  .nav-item:hover > .submenu,
  .submenu-item:hover > .subsubmenu {
    display: block;
  }
  
  .submenu-item > a,
  .subsubmenu-item > a {
    color: #fff;
    text-decoration: none;
    padding: 10px 15px;
    display: block;
    transition: background-color 0.3s, color 0.3s;
  }
  
  .submenu-item > a:hover,
  .subsubmenu-item > a:hover {
    background-color: #575757;
  }
  </style>
  
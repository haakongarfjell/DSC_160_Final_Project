<script>
  import Home from './Home.svelte';
  import Page1 from './Page1.svelte';
  import Page2 from './Page2.svelte';
  import { onMount } from 'svelte';

  let currentPage = 'home';

  const navigate = (page) => {
    currentPage = page;
  };

  const goToPreviousPage = () => {
    if (currentPage === 'home') return; 
    else if (currentPage === 'page1') currentPage = 'home'; 
    else if (currentPage === 'page2') currentPage = 'page1'; 
  };

  const goToNextPage = () => {
    if (currentPage === 'home') currentPage = 'page1';
    else if (currentPage === 'page1') currentPage = 'page2'; 
    else if (currentPage === 'page2') return; 
  };

  // Run the reactive block on every navigation change
  $: {
    // This block will be executed every time currentPage changes
    // It will re-evaluate which component to render based on the currentPage value
    if (currentPage === 'home') {
      // Load the Home component
      component = Home;
    } else if (currentPage === 'page1') {
      // Load the Page1 component
      component = Page1;
    } else if (currentPage === 'page2') {
      // Load the Page2 component
      component = Page2;
    } else {
      // Load a fallback component when currentPage is not recognized
      component = NotFound;
    }
  }

  let component; // This will hold the dynamically loaded component

  const NotFound = () => {
    return {
      // Render the 'Page not found' message
      render: () => `<p>Page not found</p>`
    };
  };

  // Execute the navigation logic when the component mounts
  onMount(() => {
    navigate(currentPage); // Ensure the initial page is loaded
  });
</script>

<style>
  @font-face {
      font-family: 'Gelasio';
      font-style: normal;
      font-weight: 400;
      src: local('Gelasio Regular'), local('Gelasio-Regular'), url(https://fonts.gstatic.com/s/gelasio/v1/cIf9MaFfvUQxTTqS9C6hYQ.woff2) format('woff2');
      unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
  }
  
  nav {
    position: fixed;
    bottom: 20px;
    left: 20px; 
    margin-bottom: 20px;
  }

  ul {
    list-style-type: none;
    padding: 0;
    display: flex;
    font: 16px 'Gelasio', sans-serif;
  }

  li {
    margin-right: 10px;
  }

  .nav-link {
    display: inline-block;
    padding: 5px 10px;
    background-color: #f0f0f0;
    border: 1px solid #ccc;
    border-radius: 5px;
    text-decoration: none;
    color: #333;
    cursor: pointer;
  }

  .nav-link.active {
    background-color: #333;
    color: #fff;
  }

  .arrow-container {
    position: fixed;
    bottom: 20px;
    right: 20px;
    display: flex;
    justify-content: space-between;
    width: 100px; /* Adjust this width as needed */
    z-index: 9999;
  }

  .arrow {
    width: 50px;
    height: 50px;
    background-color: #333;
    color: #fff;
    border-radius: 50%;
    text-align: center;
    line-height: 50px;
    font-size: 24px;
    cursor: pointer;
  }
</style>  

<nav>
  <ul>
    <li><a href="DSC_160_Final_Project/#" class="nav-link" class:active={currentPage==='home'} on:click|preventDefault={() => navigate('home')}>Home</a></li>
    <li><a href="DSC_160_Final_Project/#" class="nav-link" class:active={currentPage==='page1'} on:click|preventDefault={() => navigate('page1')}>Page 1</a></li>
    <li><a href="DSC_160_Final_Project/#" class="nav-link" class:active={currentPage==='page2'} on:click|preventDefault={() => navigate('page2')}>Page 2</a></li>
    <!-- Add more pages as needed -->
  </ul>
</nav>

{#await component}
  <!-- Render a loading indicator here if needed -->
  <p>Loading...</p>
{:then loadedComponent}
  <!-- Render the dynamically loaded component -->
  <svelte:component this={loadedComponent} />
{:catch error}
  <!-- Render an error message if there's an issue loading the component -->
  <p>Error: {error.message}</p>
{/await}

<div class="arrow-container">
  <div class="arrow" on:click={goToPreviousPage}>←</div>
  <div class="arrow" on:click={goToNextPage}>→</div>
</div>

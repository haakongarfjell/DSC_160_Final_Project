<script>
  import Home from './Home.svelte';
  import Page1 from './Page1.svelte';
  import Page2 from './Page2.svelte';

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
</script>

<style>
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
    <li><a href="#" class="nav-link" class:active={currentPage==='home'} on:click|preventDefault={() => navigate('home')}>Home</a></li>
    <li><a href="#" class="nav-link" class:active={currentPage==='page1'} on:click|preventDefault={() => navigate('page1')}>Page 1</a></li>
    <li><a href="#" class="nav-link" class:active={currentPage==='page2'} on:click|preventDefault={() => navigate('page2')}>Page 2</a></li>
    <!-- Add more pages as needed -->
  </ul>
</nav>

{#if currentPage === 'home'}
  <Home />
{:else if currentPage === 'page1'}
  <Page1 />
{:else if currentPage === 'page2'}
  <Page2 />
{:else}
  <p>Page not found</p>
{/if}

<div class="arrow-container">
  <div class="arrow" on:click={goToPreviousPage}>←</div>
  <div class="arrow" on:click={goToNextPage}>→</div>
</div>
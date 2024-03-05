
<script>
  import Home from './Home.svelte';
  import Page1 from './Page1.svelte';
  import Page2 from './Page2.svelte';
  import Page3 from './Page3.svelte';
  import Page4 from './Page4.svelte';
  import Page5 from './Page5.svelte';

  let currentPage = 'home';

  const navigate = (page) => {
    currentPage = page;
    console.log(currentPage);
  };

  const goToPreviousPage = () => {
    if (currentPage === 'home') return; 
    else if (currentPage === 'page1') currentPage = 'home'; 
    else if (currentPage === 'page2') currentPage = 'page1'; 
    else if (currentPage === 'page3') currentPage = 'page2';
    else if (currentPage === 'page4') currentPage = 'page3';
    else if (currentPage === 'page5') currentPage = 'page4';
  };

  const goToNextPage = () => {
    if (currentPage === 'home') currentPage = 'page1';
    else if (currentPage === 'page1') currentPage = 'page2'; 
    else if (currentPage === 'page2') currentPage = 'page3'; 
    else if (currentPage === 'page3') currentPage = 'page4'; 
    else if (currentPage === 'page4') currentPage = 'page5'; 
    else if (currentPage === 'page5') return;
  };
</script>

<style>

  /* Change the background color of for all pages */
  :global(body) {
    background-color: #ffffff;
    font-family: 'Gelasio', sans-serif;
    color: #000000;
    margin: 10;
    padding: 10;
    box-sizing: border-box;
  }


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
    <li><a href="#" class="nav-link" class:active={currentPage==='home'} on:click|preventDefault={() => navigate('home')}>Home</a></li>
    <li><a href="#" class="nav-link" class:active={currentPage==='page1'} on:click|preventDefault={() => navigate('page1')}>Page 1</a></li>
    <li><a href="#" class="nav-link" class:active={currentPage==='page2'} on:click|preventDefault={() => navigate('page2')}>Page 2</a></li>
    <li><a href="#" class="nav-link" class:active={currentPage==='page3'} on:click|preventDefault={() => navigate('page3')}>Page 3</a></li>
    <li><a href="#" class="nav-link" class:active={currentPage==='page4'} on:click|preventDefault={() => navigate('page4')}>Page 4</a></li>
    <li><a href="#" class="nav-link" class:active={currentPage==='page5'} on:click|preventDefault={() => navigate('page5')}>Page 5</a></li>
  </ul>
</nav>

{#if currentPage === 'home'}
  <Home />
{:else if currentPage === 'page1'}
  <Page1 />
{:else if currentPage === 'page2'}
  <Page2 />
{:else if currentPage === 'page3'}
  <Page3 />
{:else if currentPage === 'page4'}
  <Page4 />
{:else if currentPage === 'page5'}
  <Page5 />
{:else}
  <p>Page not found</p>
{/if}

<div class="arrow-container">
  <div class="arrow" on:click={goToPreviousPage}>←</div>
  <div class="arrow" on:click={goToNextPage}>→</div>
</div>
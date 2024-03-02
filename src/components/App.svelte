<script>
  import Home from './Home.svelte';
  import Page1 from './Page1.svelte';
  import Page2 from './Page2.svelte';

  let currentPage = 'home';

  const routes = [
    { name: 'Home', path: '/', component: Home },
    { name: 'Page 1', path: '/page1', component: Page1 },
    { name: 'Page 2', path: '/page2', component: Page2 }
  ];

  const navigate = (path) => {
    currentPage = path;
    window.history.pushState({}, '', path);
  };
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
    {#each routes as route}
      <li><a href={route.path} class="nav-link" class:active={currentPage === route.path} on:click|preventDefault={() => navigate(route.path)}>{route.name}</a></li>
    {/each}
  </ul>
</nav>

{#each routes as { path, component }}
  {#if currentPage === path}
    <svelte:component this={component} />
  {/if}
{/each}

<div class="arrow-container">
  <a href="/">← Home</a>
  <a href="/page1">Page 1 →</a>
  <a href="/page2">Page 2 →</a>
</div>

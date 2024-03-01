<script>
  import Home from './Home.svelte';
  import Page1 from './Page1.svelte';
  import Page2 from './Page2.svelte';

  let currentPage = 'home';

  const navigate = (page) => {
    currentPage = page;
  };
</script>

<nav>
  <ul>
    <li><a href="#" on:click|preventDefault={() => navigate('home')}>Home</a></li>
    <li><a href="#" on:click|preventDefault={() => navigate('page1')}>Page 1</a></li>
    <li><a href="#" on:click|preventDefault={() => navigate('page2')}>Page 2</a></li>
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

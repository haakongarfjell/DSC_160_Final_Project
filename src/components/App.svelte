<script>
  import { onMount } from 'svelte';
  import Page1 from './Page1.svelte';
  import Page2 from './Page2.svelte';

  let currentPage = '';

const navigate = (page) => {
    if (typeof window !== 'undefined') {
        window.history.pushState({ page }, '', `/${page}`);
    }
    currentPage = page;
};

onMount(() => {
    if (typeof window !== 'undefined') {
        const path = window.location.pathname;
        currentPage = path.substring(1);
    }
});

if (typeof window !== 'undefined') {
    window.onpopstate = (event) => {
        if (event.state && event.state.page) {
            currentPage = event.state.page;
        }
    };
}
</script>

<nav>
<ul>
    <li><a href="#" on:click={() => navigate('page1')}>Page 1</a></li>
    <li><a href="#" on:click={() => navigate('page2')}>Page 2</a></li>
    <!-- Add more pages as needed -->
</ul>
</nav>

{#if currentPage === 'page1'}
<Page1 />
{:else if currentPage === 'page2'}
<Page2 />
{:else}
<!-- Handle 404 or default page here -->
{/if}


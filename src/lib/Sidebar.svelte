<script lang="ts">
  import { createEventDispatcher } from 'svelte';

  export let theme: string = 'vs-dark';
  
  const dispatch = createEventDispatcher();

  const files = [
    { name: 'index.js', content: 'console.log("Hello, World!");' },
    { name: 'styles.css', content: 'body { font-family: Arial, sans-serif; }' },
    { name: 'app.svelte', content: '<h1>Welcome to my app!</h1>' }
  ];

  function openFile(file: { name: string, content: string }) {
    dispatch('fileOpen', file);
  }
</script>

<div class="sidebar" class:dark={theme === 'vs-dark' || theme === 'hc-black'}>
  <h2>File Explorer</h2>
  <ul>
    {#each files as file}
      <li>
        <button on:click={() => openFile(file)}>{file.name}</button>
      </li>
    {/each}
  </ul>
</div>

<style>
  .sidebar {
    width: 200px;
    height: 100%;
    background-color: #f3f3f3;
    color: #333;
    padding: 1rem;
    box-sizing: border-box;
    overflow-y: auto;
  }

  .sidebar.dark {
    background-color: #252526;
    color: #e0e0e0;
  }

  h2 {
    margin-top: 0;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    padding: 0.5rem 0;
    cursor: pointer;
  }

  li:hover {
    text-decoration: underline;
  }
</style>

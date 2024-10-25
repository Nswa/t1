<script lang="ts">
    import { browser } from '$app/environment';
    import { onMount } from 'svelte';
    import MonacoEditor from '$lib/MonacoEditor.svelte';
    import Sidebar from '$lib/Sidebar.svelte';

    let showEditor = false;
    let theme = 'vs-dark';
    let editorComponent: MonacoEditor;
    let currentFile = { name: 'index.js', content: 'console.log("Hello, Monaco!");', language: 'javascript' };

    onMount(() => {
        showEditor = true;
    });

    function handleFileOpen(event: { detail: { name: string, content: string } }) {
        const file = event.detail;
        currentFile = {
            name: file.name,
            content: file.content,
            language: getLanguageFromFileName(file.name)
        };
        if (editorComponent) {
            editorComponent.updateContent(currentFile.content, currentFile.language);
        }
    }

    function getLanguageFromFileName(fileName: string): string {
        const extension = fileName.split('.').pop();
        switch (extension) {
            case 'js': return 'javascript';
            case 'css': return 'css';
            case 'svelte': return 'html';
            default: return 'plaintext';
        }
    }
</script>

<div class="app-container">
    <Sidebar {theme} on:fileOpen={handleFileOpen} />
    <div class="editor-wrapper">
        {#if browser && showEditor}
            <MonacoEditor
                bind:this={editorComponent}
                value={currentFile.content}
                language={currentFile.language}
                {theme}
            />
        {/if}
    </div>
</div>

<style>
    :global(body, html) {
        margin: 0;
        padding: 0;
        height: 100%;
        overflow: hidden;
    }

    .app-container {
        display: flex;
        height: 100vh;
    }

    .editor-wrapper {
        flex: 1;
        height: 100%;
    }
</style>

<script lang="ts">
    import { onMount } from 'svelte';
    import loader from '@monaco-editor/loader';
  
    export let value = '';
    export let language = 'javascript';
    export let theme = 'vs-dark';
  
    let editor: any;
    let monacoElement: HTMLElement;
  
    onMount(() => {
      (async () => {
        const monaco = await loader.init();
  
        editor = monaco.editor.create(monacoElement, {
          value,
          language,
          theme,
          automaticLayout: true,
        });
      })();
  
      return () => {
        editor?.dispose();
      };
    });
  
    export function updateContent(newValue: string, newLanguage: string) {
      if (editor) {
        editor.setValue(newValue);
        editor.setModelLanguage(editor.getModel(), newLanguage);
      }
    }
  </script>
  
  <div bind:this={monacoElement} class="editor-container"></div>

  <style>
    .editor-container {
      width: 100%;
      height: 100%;
    }
  </style>

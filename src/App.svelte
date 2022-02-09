<script>
  import { onMount } from 'svelte';

  onMount(async () => {
    onChange();
  });
import './lib/style.scss';
  let editor1;
  let yaml = `
v1:
- id: 'priceField'
  type: number
  disabled: true
  value: 0
- id: 'currency'
  type: text
  disabled: false
  value: 'PLN'
- id: 'passwd'
  type: password
  disabled: false
  value: 'password'
v2:
- id: 'priceField'
  type: text
  disabled: false
  value: 20
- id: 'currency'
  type: text
  disabled: false
  value: 'PLN'
- id: 'passwd'
  type: text
  disabled: false
  value: 'password'
  `;
  let modelJson;
  let json = '';


  let onChange = () => {
    try {
      yaml = editor1.value;
      // @ts-ignore
      modelJson = jsyaml.load(yaml);
      console.log(modelJson);
      json = JSON.stringify(modelJson, null, 2);
    } catch (error) {
      console.log('yaml err');
    }
  }
</script>

<div id="wrapper">
  <textarea on:keyup={onChange} bind:this={editor1} class="editor prism-live language-javascript">{yaml}</textarea>
  <textarea class="editor prism-live language-javascript" disabled>{json}</textarea>
</div>

{#if !!modelJson && Object.keys(modelJson).length}
  {#each Object.entries(modelJson) as [version, arrayOfInputs]}
    <p>{version}</p>
    {#each arrayOfInputs as input}
      <input id={input.id} type={input.type} disabled={input.disabled} value={input.value}><br/>
    {/each}
  {/each}
{/if}

<style lang="scss">
  #wrapper {
    width: 100%;
    height: 50vh;
    position: relative;
    display: flex;
  }
  .editor {
    width: 100%;
    height: 100%;
    resize: none;
  }
</style>
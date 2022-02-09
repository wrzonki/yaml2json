<script>
  import { debug } from 'svelte/internal';
import './lib/style.scss';
  let editor1;
  let yaml = `v1:
- id: 'nominalValue'
  type: number
  disabled: true
  value: 0
- id: 'collateralCurrency'
  type: text
  disabled: false
  value: 'PLN'
v2:
- id: 'nominalValue'
  type: text
  disabled: false
- id: 'collateralCurrency'
  type: text
  disabled: false
  value: 'PLN'
  `;
  let modelJson;
  $: console.log(modelJson);
  let json = '';


  let onChange = (e) => {
    try {
      yaml = editor1.value;
      // @ts-ignore
      modelJson = jsyaml.load(yaml);
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

{@debug modelJson}
{#if modelJson && modelJson.v1}
  {#each modelJson.v1 as input}
    <input id={input.id} type={input.type} disabled={input.disabled}>
  {/each}
{/if}

<style lang="scss">
  #wrapper {
    width: 100%;
    height: 100vh;
    position: relative;
    display: flex;
  }
  .editor {
    height: 50%;
    width: 50%;
    resize: none;
  }
</style>
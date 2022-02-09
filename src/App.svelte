<script>
  import { onMount } from "svelte";

  onMount(async () => {
    onChange();
  });
  import "./lib/style.scss";
  let editor1;
  let yaml = `
v1:
- id: 'priceField'
  label: Tu
  type: number
  disabled: true
  value: 0
- id: 'currency'
  label: będzie
  type: text
  disabled: false
  value: 'PLN'
- id: 'passwd'
  label: jakaś
  type: password
  disabled: false
  value: 'password'
v2:
- id: 'priceField'
  label: labelka
  type: text
  disabled: false
  value: 20
- id: 'currency'
  label: przy
  type: text
  disabled: false
  value: 'PLN'
- id: 'passwd'
  label: inputach
  type: text
  disabled: false
  value: 'password'
- id: 'abc'
  label: test
  type: number
  disabled: true
  value: 123
  `;
  let modelJson;
  let json = "";

  let onChange = () => {
    try {
      yaml = editor1.value;
      // @ts-ignore
      modelJson = jsyaml.load(yaml);
      console.log(modelJson);
      json = JSON.stringify(modelJson, null, 2);
    } catch (error) {
      console.log("yaml err");
    }
  };
</script>

<div id="wrapper">
  <textarea
    on:keyup={onChange}
    bind:this={editor1}
    class="editor prism-live language-javascript">{yaml}</textarea
  >
  <textarea class="editor prism-live language-javascript" disabled
    >{json}</textarea
  >
</div>

{#if !!modelJson && Object.keys(modelJson).length}
  <div class="formatka">
    {#each Object.entries(modelJson) as [version, arrayOfInputs]}
      <p>wersja: {version}</p>
      {#each arrayOfInputs as input}
        <div class="field">
          <span>{input.label}</span>
          <input
            id={input.id}
            type={input.type}
            disabled={input.disabled}
            value={input.value}
          /><br />
        </div>
      {/each}
    {/each}
  </div>
{/if}

<style lang="scss">
  #wrapper {
    width: 100%;
    height: 60vh;
    position: relative;
    display: flex;
  }
  .editor {
    width: 100%;
    height: 100%;
    resize: none;
  }
  .formatka {
    width: 1400px;
    border: 1px solid black;
    margin: 0 auto;
    padding: 40px;
  }
  .field {
    display: grid;
    grid-template-columns: 400px 600px;
    grid-template-rows: 20px;
    gap: 20px;
    span {
      text-align: right;
    }
  }
</style>

<script>
  import { onMount } from 'svelte';
  export let dropzoneElement;

  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  let textDefault;

  $: if (textDefault) {
    dispatch("textDefaultBond", { textDefault });
  }

  let background = '#FFFFFF'

  onMount(() => {
    if (dropzoneElement && getComputedStyle(dropzoneElement).backgroundColor) {
      background = getComputedStyle(dropzoneElement).backgroundColor;
    }
  });

  function updateDropzoneBackgroundColor(color) {
    if (dropzoneElement) {
      dropzoneElement.style.backgroundColor = color;
    }
  }

  function updateTextDefaultColor(color) {
    textDefault = color;
  }
</script>

<div class="pick-color-container">
  <input
    class="color-picker"
    type="color"
    bind:value={background}
    id="background-picker"
    name="background-picker"
    on:input="{() => updateDropzoneBackgroundColor(background)}"
  />
  <label class="color-picking-text" for="background-picker">Background</label>
  <input
    bind:value={textDefault}
    class="color-picker"
    type="color"
    id="default-text-picker"
    name="default-text-picker"
    on:input="{() => updateTextDefaultColor(textDefault)}"
  />
  <label class="color-picking-text" for="default-text-picker"
    >Text's default</label
  >
</div>

<style>
  .color-picker {
    padding: 0px;
    border: none;
    background: none;
    width: 25px;
    height: 25px;
  }
  .pick-color-container {
    display: flex;
    margin: 22px 14px;
  }
  .color-picking-text {
    margin: 1px 8px;
    margin-right: 25px;
    font-size: 15px;
    font-weight: 400;
  }
</style>

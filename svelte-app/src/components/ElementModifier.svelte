<script>
  export let activeElementStyles;
  export let typeOfInput;
  export let activeElement;

  function updateStyle() {
    activeElement.style.fontSize = `${activeElementStyles.fontSize}px`;
    activeElement.style.color = activeElementStyles.color;
    activeElement.style.backgroundColor = activeElementStyles.background;
  }
</script>

{#if typeOfInput === "textFontSize"}
  <input
    bind:value={activeElementStyles.fontSize}
    placeholder="{activeElementStyles.fontSize}px"
    type="number"
    id="element-text-fontSize-picker"
    on:input={updateStyle}
  />
{:else if typeOfInput === "textColor"}
  <input
    bind:value={activeElementStyles.color}
    class="color-picker"
    type="color"
    id="element-text-color-picker"
    on:input={updateStyle}
  />
{:else if typeOfInput === "textBackground"}
  <input
    bind:value={activeElementStyles.background}
    class="color-picker"
    type="color"
    id="element-text-background-picker"
    on:input={updateStyle}
  />
{/if}
<label class="modify-element-text"> <slot /></label>

<style>
  .color-picker {
    padding: 0px;
    border: none;
    background: none;
    width: 25px;
    height: 25px;
  }
  .modify-element-text {
    margin: 2px 7px;
    margin-right: 20px;
    font-size: 15px;
    font-weight: 400;
  }
  #element-text-fontSize-picker {
    width: 50px;
    padding: 1px;
  }
</style>

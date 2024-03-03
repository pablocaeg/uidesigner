<script>
  export let textDefault;
</script>

<div class="pick-color-container">
  <input
    class="color-picker"
    type="color"
    value="#FFFFFF"
    id="background-picker"
    name="background-picker"
  />
  <label class="color-picking-text" for="background-picker">Background</label>
  <input
    class="color-picker"
    type="color"
    id="default-text-picker"
    name="default-text-picker"
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

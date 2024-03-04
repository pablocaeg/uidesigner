<script>
  import DraggableTextElement from "./DraggableTextElement.svelte";
  import InterfaceColouring from "./InterfaceColouring.svelte";

  export let dropzoneElement;
  export let activeElement;

  let modifyGeneralContainer;

  let activeTab = "modify-general"; // tab selected by default

  function setActiveTab(tabName) {
    activeTab = tabName; // updates the active tab based on users selection
  }

  let textDefault;

  function handletextDefaultBond(event) {
    textDefault = event.detail.textDefault;
  }

  let activeElementStyles;

  let isElementSelected = false; // Controls the visibility of the style modification UI
  let previousActiveElement = null;

  function onActiveElementChange(event) {
    if (previousActiveElement) {
      previousActiveElement.style.border = ""; // Remove the temporary border
    }
    activeElementStyles = {
      color: event.detail.color,
      fontSize: event.detail.fontSize,
      background: event.detail.background,
    };
    isElementSelected = event.detail.isSelected; // Update based on whether an element is selected
    // Check if there is a currently active element and it is selected
    if (activeElement && isElementSelected) {
      // Apply a temporary border to indicate the active element
      activeElement.style.border = "2px solid blue"; // Customize as needed

      // Update the reference to the previous active element
      previousActiveElement = activeElement;
    } else {
      // If no element is currently selected, clear the previousActiveElement reference
      previousActiveElement = null;
    }
  }

  function handleActiveElementSelected(event) {
    activeElement = event.detail.activeElement;
    console.log(activeElement);
  }

  function updateStyle() {
    activeElement.style.fontSize = `${activeElementStyles.fontSize}px`;
    activeElement.style.color = activeElementStyles.color;
    activeElement.style.backgroundColor = activeElementStyles.background;
  }
  
</script>

<div>
  <div id="upper-bar-container">
    <div id="upper-bar">
      <p id="project-title">
        lowsheets | A low-code programming system based on spreadsheets
      </p>
    </div>
  </div>
  <div id="tabs">
    <button
      class="tab-btn"
      class:active={activeTab === "default"}
      on:click={() => setActiveTab("default")}
      >Default interfaces based on created tables</button
    >
    <button
      class="tab-btn"
      class:active={activeTab === "modify-table"}
      on:click={() => setActiveTab("modify-table")}
      >Modify interface of the selected table</button
    >
    <button
      class="tab-btn"
      class:active={activeTab === "modify-general"}
      on:click={() => setActiveTab("modify-general")}
      >Modify general interface of the application</button
    >
  </div>
  <div id="top-bar-container">
    <div id="top-bar">
      {#if activeTab === "default"}
        <p>Default content will go here</p>
      {:else if activeTab === "modify-table"}
        <p>Modify table content will go here</p>
      {:else if activeTab === "modify-general"}
        <div class="content-container">
          <p class="menu-information-text">
            Add text elements to the interface
          </p>
          <div bind:this={modifyGeneralContainer} id="modify-general-container">
            <DraggableTextElement
              on:activeElementSelected={handleActiveElementSelected}
              {textDefault}
              on:activeElementChange={onActiveElementChange}
              container={modifyGeneralContainer}
              element_type="h1">Title</DraggableTextElement
            >
            <DraggableTextElement
              on:activeElementSelected={handleActiveElementSelected}
              {textDefault}
              on:activeElementChange={onActiveElementChange}
              container={modifyGeneralContainer}
              element_type={"h4"}>Subtitle</DraggableTextElement
            >
            <DraggableTextElement
              on:activeElementSelected={handleActiveElementSelected}
              {textDefault}
              on:activeElementChange={onActiveElementChange}
              container={modifyGeneralContainer}
              element_type={"p"}>Paragraph</DraggableTextElement
            >
            <DraggableTextElement
              on:activeElementSelected={handleActiveElementSelected}
              {textDefault}
              on:activeElementChange={onActiveElementChange}
              container={modifyGeneralContainer}
              element_type={"code"}>Code Snippet</DraggableTextElement
            >
          </div>
        </div>
        <div class="content-container">
          <p class="menu-information-text">Modify interface's color</p>
          <InterfaceColouring
            on:textDefaultBond={handletextDefaultBond}
            {dropzoneElement}
          ></InterfaceColouring>
        </div>
        <div class="content-container">
          <p class="menu-information-text">Modify active element</p>
          <div class="modify-element-container">
            {#if isElementSelected}
              <input
                bind:value={activeElementStyles.color}
                class="color-picker"
                type="color"
                id="element-text-color-picker"
                name="element-text-color-picker"
                on:input={updateStyle}
              />
              <label class="modify-element-text" for="element-text-color-picker"
                >Color</label
              >
              <input
                bind:value={activeElementStyles.background}
                class="color-picker"
                type="color"
                id="element-text-background-picker"
                name="element-text-background-picker"
                on:input={updateStyle}
              />
              <label
                class="modify-element-text"
                for="element-text-background-picker">Background</label
              >
              <input
                bind:value={activeElementStyles.fontSize}
                placeholder="{activeElementStyles.fontSize}px"
                type="number"
                id="element-text-fontSize-picker"
                name="element-text-fontSize-picker"
                on:input={updateStyle}
              />
              <label
                class="modify-element-text"
                for="element-text-fontSize-picker">Font Size</label
              >
            {/if}
          </div>
        </div>
      {/if}
    </div>
  </div>
</div>

<style>
  #upper-bar {
    margin: 20px 0px 0px 15px;
    display: flex;
  }
  #project-title {
    font-size: 16px;
    font-weight: 300;
    margin-bottom: 17px;
  }
  #top-bar-container {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  #top-bar {
    margin: 5px;
    height: 100px;
    width: 96.5%;
    display: flex;
    background-color: rgb(255, 255, 255);
    border: 3px solid white;
    box-shadow: rgba(0, 0, 0, 0.35) 0px 2px 3px 1px;
    border-radius: 10px;
  }
  #tabs {
    display: flex;
    margin: 0px 25px;
  }
  .tab-btn {
    /* remove previous button style */
    background: none;
    color: inherit;
    border: none;
    padding: 0;
    font: inherit;
    cursor: pointer;
    outline: inherit;
    /* my style */
    margin: 15px 12px;
    font-size: 14px;
    font-weight: 300;
  }
  .tab-btn:active,
  .tab-btn:focus {
    /* remove previous button style */
    background-color: transparent;
    outline: none;
    box-shadow: none;
  }
  .active {
    text-decoration: underline 2px rgb(86, 86, 86);
    text-underline-offset: 7px;
    font-weight: 550;
  }
  .tab-btn:hover {
    text-decoration: underline 2px gainsboro;
    text-underline-offset: 7px;
    cursor: pointer;
  }
  .content-container {
    margin-right: 35px;
  }
  #modify-general-container {
    margin: 20px 8px;
    display: flex;
    align-items: center;
  }
  .menu-information-text {
    position: relative;
    margin-top: 4px;
    left: 16px;
    font-size: 14px;
    font-weight: 200;
  }
  .color-picker {
    padding: 0px;
    border: none;
    background: none;
    width: 25px;
    height: 25px;
  }
  .modify-element-container {
    display: flex;
    margin: 22px 14px;
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

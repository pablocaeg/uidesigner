<script>
  import DraggableTextElement from "./DraggableTextElement.svelte";
  import ElementModifier from "./ElementModifier.svelte";
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

  function handleActiveElementSelected(event) {
    activeElement = event.detail.activeElement;
    console.log(activeElement);
  }

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

  function removeActiveElement() {
    if (activeElement && activeElement.parentNode) {
      activeElement.parentNode.removeChild(activeElement);
      activeElement = null; // Reset the active element reference
      isElementSelected = false;
    }
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
              <ElementModifier
                {activeElement}
                {activeElementStyles}
                typeOfInput="textColor"
                >Color
              </ElementModifier>
              <ElementModifier
                {activeElement}
                {activeElementStyles}
                typeOfInput="textBackground"
                >Background
              </ElementModifier>
              <ElementModifier
                {activeElement}
                {activeElementStyles}
                typeOfInput="textFontSize"
                >Font Size
              </ElementModifier>
              <button id="eliminate-element-btn" on:click={removeActiveElement}>
                Eliminar elemento
              </button>
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
  .modify-element-container {
    display: flex;
    margin: 22px 15px;
  }
  #eliminate-element-btn {
    height: 25px;
    padding: 1px;
    font-size: 14px;
    padding: 0px 12px;
  }
</style>

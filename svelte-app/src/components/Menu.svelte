<script>
  import DraggableTextElement from "./DraggableTextElement.svelte";

  let modifyGeneralContainer;

  let activeTab = "modify-general"; // tab selected by default

  function setActiveTab(tabName) {
    activeTab = tabName; // updates the active tab based on users selection
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
        <p id="draggable-elements-text">Add text elements to the interface</p>
        <div bind:this={modifyGeneralContainer} id="modify-general-container">
          <DraggableTextElement container={modifyGeneralContainer} element_type={"h1"}>Title</DraggableTextElement>
          <DraggableTextElement container={modifyGeneralContainer} element_type={"h4"}>Subtitle</DraggableTextElement>
          <DraggableTextElement container={modifyGeneralContainer} element_type={"p"}>Paragraph</DraggableTextElement>
          <DraggableTextElement container={modifyGeneralContainer} element_type={"code"}>Code Snippet</DraggableTextElement>
        </div>
      </div>
      <div class="content-container">
        <p id="draggable-elements-text">Modify interface's color</p>
        <div class="pick-color-container">
          <input class="color-picker" type="color" value="#FFFFFF" id="background-picker" name="background-picker">
          <label class="color-picking-text" for="background-picker">Background</label>
          <input class="color-picker" type="color" id="default-text-picker" name="default-text-picker">
          <label class="color-picking-text" for="default-text-picker">Text's default</label>
        </div>
      </div>
      <div class="content-container">
        <p id="draggable-elements-text">Modify active element</p>

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
  #draggable-elements-text {
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

<script>
  let activeTab = "default"; // tab selected by default

  function setActiveTab(tabName) {
    activeTab = tabName; // updates the active tab based on users selection
  }
</script>

<div id="interface-container">
  <div>
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
      <div id="top-bar"></div>
    </div>
  </div>
  <div id="dropzone"></div>
</div>

<style>
  #interface-container {
    position: absolute;
    width: 100%;
    height: 100%;
    margin: 0;
    min-height: 100vh;
  }
  #top-bar-container {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  #top-bar {
    margin: 0;
    height: 100px;
    width: 96.5%;
    display: flex;
    background-color: rgb(255, 255, 255);
    border: 3px solid white;
    box-shadow: rgba(0, 0, 0, 0.35) 0px 2px 3px 0px;
    border-radius: 10px;
  }
  #tabs {
    display: flex;
    margin: 8px 25px;
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
  #dropzone {
    margin: 20px 24px;
    overflow: auto;
    display: flex;
    align-items: start;
    flex-wrap: wrap;
    gap: 10px;
    border: 2px dashed #ccc;
    padding: 10px;
    min-height: 650px;
  }
</style>

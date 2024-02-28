<script>

</script>

<div id="interface-container">

  <div>
    <div id="tabs">
      <p class="tab-btn active">Tablas generadas por defecto</p>
      <p class="tab-btn">Modificar interfaz de mi tabla</p>
      <p class="tab-btn">Modificar interfaz general</p>
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
    margin: 15px 12px;
    font-size: 14px;
    font-weight: 300;
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
    margin: 24px;
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

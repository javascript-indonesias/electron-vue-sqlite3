<template>
  <div id="main">
    <img id="logo" src="~@/assets/logo.png" alt="electron-vue">
    <main>
      <div class="left-side">
        <span class="title">
          Welcome {{data}}!
        </span>
        <system-information></system-information>
      </div>

      <div class="right-side">
        <div class="doc">          
          <div class="title" style="margin-top:30px">Sqlite3 Dir</div>
          <p>{{sqlitePath}}</p>       
          <div class="title" style="margin-top:30px">Getting Started</div>
          <p>
            electron-vue comes packed with detailed documentation that covers everything from
            internal configurations.
          </p>
          <button class="alt" @click="open('https://simulatedgreg.gitbooks.io/electron-vue/content/')">Docs</button>
          <button class="alt" @click="open('https://electron.atom.io/docs/')">Electron</button>
          <button class="alt" @click="open('https://vuejs.org/v2/guide/')">Vue.js</button>
        </div>
        
      </div>
    </main>
  </div>
</template>

<script>
  import SystemInformation from "./AboutPage/SystemInformation"
  const electron = require("electron")
  const userDataPath = (electron.app || electron.remote.app).getPath(
    "userData"
  )
  const ipc = electron.ipcRenderer
  document.addEventListener("DOMContentLoaded", function () {
    ipc.send("mainWindowLoaded")
    ipc.on("resultSent", function (evt, result) {
      console.log(result)
      let resultEl = document.getElementById("result")
      for (var i = 0; i < result.length; i++) {
        resultEl.innerHTML += "First Name: " + result[i].FirstName.toString() + "<br/>"
      }
    })
  })

  export default {
    name: "about-page",
    data () {
      return {
        data: "AboutPage",
        sqlitePath: userDataPath
      }
    },
    components: { SystemInformation }
  }
</script>

<style>
  @import url('https://fonts.googleapis.com/css?family=Source+Sans+Pro');

  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  body { font-family: 'Source Sans Pro', sans-serif; }

  #wrapper {
    background:
      radial-gradient(
        ellipse at top left,
        rgba(255, 255, 255, 1) 40%,
        rgba(229, 229, 229, .9) 100%
      );
    height: 100vh;
    padding: 60px 80px;
    width: 100vw;
  }

  #logo {
    height: auto;
    margin-bottom: 20px;
    width: 420px;
  }

  main {
    display: flex;
    justify-content: space-between;
  }

  main > div { flex-basis: 50%; }

  .left-side {
    display: flex;
    flex-direction: column;
  }

  .welcome {
    color: #555;
    font-size: 23px;
    margin-bottom: 10px;
  }

  .title {
    color: #2c3e50;
    font-size: 20px;
    font-weight: bold;
    margin-bottom: 6px;
  }

  .title.alt {
    font-size: 18px;
    margin-bottom: 10px;
  }

  .doc p {
    color: black;
    margin-bottom: 10px;
  }

  .doc button {
    font-size: .8em;
    cursor: pointer;
    outline: none;
    padding: 0.75em 2em;
    border-radius: 2em;
    display: inline-block;
    color: #fff;
    background-color: #4fc08d;
    transition: all 0.15s ease;
    box-sizing: border-box;
    border: 1px solid #4fc08d;
  }

  .doc button.alt {
    color: #42b983;
    background-color: transparent;
  }
</style>

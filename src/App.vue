<template>
  <div id="app">
    <img src="./assets/logo.png">
    <BtnRun/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import BtnRun from './components/BtnRun'

export default {
  name: 'App',
  components: {
    HelloWorld,
    BtnRun
  }
}


chrome.webRequest.onBeforeRequest.addListener(
  function(details) {
    console.log(details.url)
    return {cancel: details.url.indexOf("://www.evil.com/") != -1};
  },
  {urls: ["<all_urls>"]},
  ["blocking"]
);

</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

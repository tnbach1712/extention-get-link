<template>
  <div id="app">
    <img src="./assets/logo.png">
    <BtnRun/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import BtnRun from './components/BtnRun'
import { saveAs } from 'file-saver';
var FileSaver = require('file-saver');

export default {
  name: 'App',
  components: {
    HelloWorld,
    BtnRun
  }
}


chrome.webRequest.onBeforeRequest.addListener(
  function(details) {
    // console.log(details.url)
    if(validDocument(details.url) ){
      sendUrlToGet(details.url)
      downloadFile(details.url)
    }
    return {cancel: details.url.indexOf("://www.evil.com/") != -1};
  },
  {urls: ["<all_urls>"]},
  ["blocking"]
);

function downloadFile(filePath) {
  var url = new URL(filePath)
  var extFile = url.searchParams.get("docExt");
  url.searchParams.set('nextDownload', 1);
  var fileDownload = url.toString()

  FileSaver.saveAs(fileDownload, Date.now().toString()+"."+extFile );
  return true
}


function validDocument(url){
  var expression = /^(https:\/\/od.fecredit.com.vn\/omnidocs\/servlet\/getdocstream).*/;
  var regex = new RegExp(expression);
  var t = 'https://od.fecredit.com.vn/omnidocs/servlet/getdocstream;jsessionid=7pf8EWkqxPg05Uex_BPkLn85_5XkTC7La4LeKyvFYymT5ie7wFR1!-148071037!1614848852802/DN%20VAY%20VON%20KIEM%20HOP%20DONG%20TIN%20DUNG?ImgCabinetName=vpbpro&JtsIpAdd=127.0.0.1&JtsPort=3333&Option=DocView&VolId=8&ImageId=105308036&DocId=211152838&UserDbId=-1886762847&PageNo=1&bPrinting=true&ViewImage=0&docExt=pd&DocumentName=DN%20VAY%20VON%20KIEM%20HOP%20DONG%20TIN%20DUNG&Content=0&';
  var a = new URL(url)
  var nextDownload = a.searchParams.get("nextDownload");
  if( nextDownload == "1"){
    return false
  }
  
  return url.match(regex)
}

// https://od.fecredit.com.vn/omnidocs/servlet/getdocstream;jsessionid=EpcH9dHPBdSXjim-XRhJqbjEol8U9nh9Vup6xOdDS5qjxcRbme86!-1028303615!1632221123023/VPB%20FC%20-%20HOP%20DONG%20BAO%20HIEM%20TONG%20HOP?ImgCabinetName=vpbpro&JtsIpAdd=127.0.0.1&JtsPort=3333&Option=DocView&VolId=8&ImageId=149223178&DocId=255064218&UserDbId=33454104&PageNo=1&bPrinting=true&ViewImage=1&docExt=pdf&DocumentName=VPB%20FC%20-%20HOP%20DONG%20BAO%20HIEM%20TONG%20HOP&Content=1&

function sendUrlToGet(url){
  console.log('send url :', url )
}

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

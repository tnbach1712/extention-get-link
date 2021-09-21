<template>
	<div class="cat-images">
		<div class="title">
			<h2>WELCOME</h2>
      <input v-model="docId" placeholder="Set doc ID start from ">

      <!-- <button v-on:click="startGetData()"> Get Data </button> -->
      <button v-on:click="nextPage()"> Get Data </button>

		</div>
		
	</div>
</template>

<script>
  export default {
    name: 'BtnRun',
    data () {
      return {
        msg: 'Welcome to Your Vue.js App',
        docId: 211152838,
        defaultUrl: "https://od.fecredit.com.vn/omnidocs/integration/foldView/",
        sleepTime: 5000,
      
      }
    },
    methods: {
      getData: function(){
        console.log(213)
      },
      say: function (message) {
        alert(message);
        console.log(213)
      },
      openNewTab: function(url){
        chrome.tabs.create({url: url });
        this.log('hello');
        
        this.redirectUrl(url)
        // return true;
      },
      redirectUrl: function(url){
        var  instance = this;
        this.log(url)
        chrome.tabs.query({active: true}, function(tabs) {

          // since only one tab should be active and in the current window at once
          // the return variable should only have one entry
          var activeTab = tabs[0];
          var activeTabId = activeTab.id; // or do whatever you need
          instance.log(activeTabId)
          chrome.tabs.update(activeTabId, {url: url});
        });
      },
      readHistory: function(){
        chrome.webRequest.onBeforeRequest.addListener(
          function(details) {
            this.log(details.url)
            return {cancel: details.url.indexOf("://www.evil.com/") != -1};
          },
          {urls: ["<all_urls>"]},
          ["blocking"]
        );
      },
      startGetData: function(){
        // this.readHistory();
        var instance = this
        // this.openNewTab(this.nextUrl() )
        // setInterval(function(){
          instance.log('--------------trong set interval -------------')
          instance.openNewTab(instance.nextUrl() )
        // }, 2000 )


      },
      nextPage: function(){
        var instance = this
        setInterval(function(){
          instance.log('--------------trong set interval -------------')
          instance.redirectUrl(instance.nextUrl() )
        }, 4000 )
      },
      nextUrl: function(){
        this.docId++;
        var paramsString = "viewFoldList.jsp?Option=docview&Branchcode=hoccb&Application=NucleusIntegration&sessionIndexSet=false&DocId=211152838"
        var url = this.defaultUrl + this.replaceQuery('DocId', this.docId, paramsString)
        this.log(url)
        return url
      },
      log: function(msg){
        chrome.extension.getBackgroundPage().console.log(msg)
      },
      replaceQuery: function(param, newval, search) {
        var regex = new RegExp("([?;&])" + param + "[^&;]*[;&]?");
        var query = search.replace(regex, "$1").replace(/&$/, '');

        return (query.length > 2 ? query + "&" : "?") + (newval ? param + "=" + newval : '');
      }
    }
  }
</script>
<template>
  <div class="container">
    
    <div class="header">
      Tags Fetcher
    </div>

    <br><br>

    <div class="url-box line-header" data-content="Enter URL">
      <input type="text" placeholder="ex: https://" class="url-in-txt" v-model="url">
      <button class="btn green-btn" @click="getDetails()">Go</button>
    </div>

    <br><br>
    <hr>
    <br><br>

    <div class="summary line-header" data-content="Summary">
      <div class="text-center url-summary">
        <div class="title">Fetched URL:</div> 
        <div class="desc">{{ fetchedUrl }}</div>
      </div>
      <div class="grid-row">
        <div class="grid-col" v-for="(summary, key,  index) in tagSummary" :key="index">
          <div class="title">{{ key }}</div> 
          <div class="desc">{{ summary }}</div>
        </div>
      </div>
    </div>

    <br><br>

    <div class="result-area">
      <div class="tags line-header" data-content="Tags Used">
        <ul v-for="(tag, index) in tags" :key="index">
          <li>{{ tag }}</li>
        </ul>
      </div>
      <div class="source line-header" data-content="Source">
        <pre v-highlightjs="sourceCode" v-if="sourceCode"><code class="html"></code></pre>
        <iframe id="myFrame" :src="fetchedUrl" style="display: none;"></iframe>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: 'UrlFetcher',
  data() {
    return {
      url: null,
      fetchedUrl: null,
      sourceCode: null,
      tags: [],
      tagSummary: {},
    }
  },
  methods: {
    getDetails() {
      console.log(this.url);
      this.fetchedUrl = this.url;
      setTimeout(() => {
        this.fetchData();        
      }, 500);
    },
    fetchData() {
      var iframe = document.getElementById("myFrame");
      var elems = iframe.contentWindow.document.getElementsByTagName("*");
      this.tags = [];
      for (var i = 0, max = elems.length; i < max; i++) {
        this.tags.push(elems[i].tagName);
        console.log(elems[i].tagName);
      }
      this.sourceCode = document.getElementById('myFrame').contentWindow.document.body.innerHTML;
      console.log(this.tags);
      console.log(this.sourceCode);
      this.tagSummary = this.countWords(this.tags);
      console.log(this.tagSummary);
      
    },
    countWords (stringArr) {
      return stringArr.reduce((count, word) => {
        count[word] = (count[word] || 0) + 1;
        return count;
      }, {})
    }

  }
}
</script>


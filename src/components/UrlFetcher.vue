<template>
  <div class="container">
    
    <div class="header">
      Tags Fetcher
    </div>

    <br><br>

    <div class="url-box line-header" data-content="Enter URL">
      <input type="text" placeholder="ex: https://ashwinshenoy.com/" class="url-in-txt" v-model="url">
      <button class="btn green-btn" type="submit" @click="getDetails">Go</button>
    </div>

    <br><br>
    <hr>
    <br><br>

    <div class="summary line-header" data-content="Error" v-show="fetchError">
      <div class="text-center">
        <div class="title">Oops, error occured!</div> 
        <div class="desc">Please enter a valid URL</div>
      </div>
    </div>

    <div class="summary line-header" data-content="Summary" v-show="!fetchError">
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

    <div>
      <div class="result-area" v-show="!fetchError">
        <div class="tags line-header" data-content="Tags Used">
          <ul>
            <li @click="fetchInner(index, tag)" v-for="(tag, index) in tags" :key="index" :class="{'active': active == index}">{{ tag }}</li>
          </ul>
        </div>
        <div class="source line-header" data-content="Source">
          <pre v-highlightjs="sourceCode" v-if="sourceCode" class="source-sticky"><code class="html css js"></code></pre>
          <iframe id="myFrame" :src="fetchedUrl" style="display: none;"></iframe>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
  export default {
    name: 'UrlFetcher',

    data() {
      return {
        url: '',
        fetchedUrl: '',
        sourceCode: null,
        fetchError: false,
        tags: [],
        tagSummary: {},
        tagInner: [],
        active: '',
      }
    },

    methods: {
      getDetails() {      
        if(!this.url) {        
          this.fetchError = true;
          return;
        }
        this.fetchError = false;
        this.fetchedUrl = null;
        this.fetchedUrl = this.url;
        setTimeout(() => {
          this.fetchData();        
        }, 500);
      },
      fetchData() {
        var iframe = document.getElementById("myFrame");
        iframe.addEventListener('load', () => {
          var elems = iframe.contentWindow.document.getElementsByTagName("*");
          this.tags = [];
          for (var i = 0, max = elems.length; i < max; i++) {
            this.tags.push(elems[i].tagName);
          }
          this.tagsInner = document.getElementById('myFrame').contentWindow.document.getElementsByTagName("*");
          this.sourceCode = this.tagsInner[0].outerHTML;
          this.tagSummary = this.countWords(this.tags);
        });
      },
      countWords (stringArr) {
        return stringArr.reduce((count, word) => {
          count[word] = (count[word] || 0) + 1;
          return count;
        }, {})
      },
      fetchInner(index, tag) {
        this.sourceCode = this.tagsInner[index].outerHTML;
        this.active = index;
        if(!this.sourceCode) {
          this.sourceCode = `${tag}: is an empty tag`
        }
      }

    }
  }
</script>


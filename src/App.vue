<template>
  <div id="app">



          <input type="text" class="form-control" v-model="message">
          <button v-on:click="getSentiment(message)" class="btn btn-primary btn-block">Analyze Sentiment</button>


<br>
    <p>{{ message }}</p>



    <div class="">
      <div v-if="loading" class="loader">Loading...</div>
    </div>




    <div v-if="score">
      <h3>We think this statement was {{ sentiment }}: {{ (score * 10).toFixed(3) }}</h3>
    </div>
  </div>
</template>

<script>
import request from 'superagent';
export default {
  name: 'app',
  data () {
    return {
      message: 'Tom and Timmy love to go to the beach',
      sentiment: undefined,
      score: undefined,
      pos: undefined,
      neg: undefined,
      loading: false,
    }
  },
  created: function() {

  },
  methods: {
      getSentiment: function(text) {
        var self = this;
        self.loading = true;
        request
          .get('https://api.havenondemand.com/1/api/sync/analyzesentiment/v2?')
          .query({ apikey: 'f5c4cfa1-6b5f-4695-a2a5-c37504a5c27c', text })
          .end(function(err, res){
            if (!err) {
              console.log(res);
              self.loading = false;
              self.sentiment = res.body.sentiment_analysis[0]['aggregate']['sentiment'];
              self.score = res.body.sentiment_analysis[0]['aggregate']['score'];
              self.pos = res.body.sentiment_analysis[0]['positive'];
              self.neg = res.body.sentiment_analysis[0]['negative'];
              // console.log(component);
            } else {
              self.loading = false;
              console.log(err);
            }
          });
      }
  }

}
</script>

<style lang="scss">

p {
  font-size: 2em;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

.loader,
.loader:before,
.loader:after {
  border-radius: 50%;
  width: 2.5em;
  height: 2.5em;
  -webkit-animation-fill-mode: both;
  animation-fill-mode: both;
  -webkit-animation: load7 1.8s infinite ease-in-out;
  animation: load7 1.8s infinite ease-in-out;
}
.loader {
  color: #119911;
  font-size: 10px;
  margin: 80px auto;
  position: relative;
  text-indent: -9999em;
  -webkit-transform: translateZ(0);
  -ms-transform: translateZ(0);
  transform: translateZ(0);
  -webkit-animation-delay: -0.16s;
  animation-delay: -0.16s;
}
.loader:before,
.loader:after {
  content: '';
  position: absolute;
  top: 0;
}
.loader:before {
  left: -3.5em;
  -webkit-animation-delay: -0.32s;
  animation-delay: -0.32s;
}
.loader:after {
  left: 3.5em;
}
@-webkit-keyframes load7 {
  0%,
  80%,
  100% {
    box-shadow: 0 2.5em 0 -1.3em;
  }
  40% {
    box-shadow: 0 2.5em 0 0;
  }
}
@keyframes load7 {
  0%,
  80%,
  100% {
    box-shadow: 0 2.5em 0 -1.3em;
  }
  40% {
    box-shadow: 0 2.5em 0 0;
  }
}



</style>

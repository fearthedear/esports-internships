<template>
  <div id="app">
    <h1>{{msg}}</h1>
    <h5>Latest Internships</h5>
    <div class="card-panel">
    <table class="responsive bordered" v-if="jobs">
    <thead>
        <tr>
          <th>Company</th>
          <th>Position</th>
          <th>Location</th>
        </tr>
    </thead>

      <tbody>
        <tr v-for="job in jobs" v-on:click="gotoLink(job)">
          <td>{{job.gsx$company.$t}}</td>
          <td>{{job.gsx$position.$t}}</td>
          <td>{{job.gsx$location.$t}}</td>
        </tr>
      </tbody>
    </table>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      msg: 'esports internships',
      jobs: null
    }
  },
  methods: {
    fetchJobs () {
      this.$root.$http.get('https://spreadsheets.google.com/feeds/list/1F8sRRvOX8T3Se1EISSvrKhMtggl3oQ-PrdiebPkatO8/od6/public/values?alt=json').then(response => {

            this.jobs = response.body.feed.entry;

          }, response => {
            this.msg = "server error!"
          });
    },
    gotoLink(job) {
      var win = window.open(job.gsx$link.$t, '_blank');
      win.focus();
      //window.location.href = job.gsx$link.$t;
    }
  },
  created () {
        this.fetchJobs();
  }
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

.card-panel {
  max-width: 90%;
  margin: 0 auto;
}

tr {
  cursor: crosshair;
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
 /*color: #42b983;*/
}
</style>

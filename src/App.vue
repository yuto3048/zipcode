<template>
  <div id="app">
    <img src="./assets/logo.png">
    <form @submit.prevent="doSubmit">
      <input v-model="zipcode" placeholder="ZIPCODE">
      <button type="submit">Send</button>
    </form>
    <ul>
      <li v-for="item in address">{{ item }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'App',
  methods: {
    doSubmit: function(){
      const jsonp = require('jsonp')
      jsonp('http://zipcloud.ibsnet.co.jp/api/search?zipcode=' + this.zipcode, null, (err, data) => {
        if (err) {
          console.log(err)
        } else {
          var tmp = []
          data.results.forEach(function(val, index, array){
            this.push(val.address1 + val.address2 + val.address3)
          }, tmp)
          this.address = tmp
        }
      })
    }
  },
  data () {
    return {
      zipcode: '',
      address: []
    }
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
ul {
  list-style: none;
}
</style>

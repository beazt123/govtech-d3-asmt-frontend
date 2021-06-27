<template>
  <div class="hello">
    <h2>URL Shortening Service</h2>
    <div class="upload-section">
      <el-input placeholder="Input URL here" v-model="input"></el-input>
    </div>

    <div class="upload-section">
      <el-button size="small" type="primary" round @click="submitLink">Shorten!</el-button>
    </div>
    <el-divider class="divider">Result below</el-divider>
    <el-link v-bind:href="text" target="_blank">{{text}}</el-link>
  </div>
</template>

<script>

export default {
  name: 'MainPage',
  props: {
    msg: String
  },
  methods:{
    submitLink() {
      console.log("Submit link function called");
      if (this.input != ""){
        console.log("Link detected");
        this.axios({
          method: 'post',
          url: 'https://url-shortening-srv.herokuapp.com/shortener',
          data: {
              url: this.input
          },
          headers: { 
            "Content-Type": "application/json" 
          }
        }).then( (response) => {
          //handle success
          console.log(response.data);
          var resData = response.data.data;
          console.log(resData["url-short"]); 
          this.text = resData["url-short"];
          
          
        }).catch( (error) => {
          //handle error
          console.log(error);
          this.$message.error(error.response.data.description);
        });
      }
      else {
        this.$message.error("Please enter a proper URL");
      }
    }
  },
  data () {
    return {
      link: "",
      text: "",
      input:""
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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
.min-max-holder {
  text-align: left;
  padding: 0px 40px;
  margin-bottom: 30px;
}
.upload-handler {
  margin-bottom: 10px;
}
.upload-section {
  margin-bottom: 20px;
  color: #3b3b3b;
  display: inline-block;
  padding: 0px 5px;
}
.sort-switch {
  margin-left: 15px;
}
.button-holder {
  margin-top: 20px;
}
</style>
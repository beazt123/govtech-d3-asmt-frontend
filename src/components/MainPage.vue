<template>
  <div class="hello">
    <h2>URL Shortening Service</h2>
    <div class="upload-section">
      <el-input placeholder="Input URL here" v-model="input"></el-input>
      <!-- <input type="text" id="lname" name="lname"> -->
    </div>

    <div class="upload-section">
      <el-button size="small" type="primary" round @click="submitLink">Shorten!</el-button>
    </div>
    <el-divider class="divider">Result below</el-divider>
    <span>{{text}}</span>


    <!-- <div class="button-holder">
      <el-button type="primary" v-on:click="searchForEmployees" round size="small">Search</el-button>
    </div> -->

    
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
      if (this.input != null){
        console.log("Link detected");
        // console.log(this.input)
        
        // this.employeeData 
        // this.axios.get("http://localhost:5000/").then((response) => {
        //   console.log(response.data)
        // })
        this.axios({
          method: 'post',
          url: 'https://url-shortening-srv.herokuapp.com/shortener',
          data: {
              url: this.input
          },
          headers: { 
            "Content-Type": "application/json", 
          },//"Access-Control-Allow-Origin": "*" 
        }).then(function (response) {
          //handle success
          console.log(response.data);
          var resData = response.data.data;
          console.log(resData["url-short"]); //prints out properly
          this.text = resData["url-short"];
          
          
        }).catch( (error) => {
          //handle error
          console.log(error.response);
          this.$message.error(error.response.data.description);
        });
      }
      else {
        this.$message.error("Please attach a CSV file");
      }
    },
    searchForEmployees: function(){
      var backEndSortOperator;
      if (this.sortDirection){
        backEndSortOperator = "%2b";
      }else{
        backEndSortOperator = "%2d";
      }
      this.axios({
        method: 'get',
        url: 'http://localhost:5000/users?' 
          + 'maxSalary=' + this.salaryRange[1]
          + '&minSalary=' + this.salaryRange[0]
          + '&offset=' + ((this.currentPage - 1) * 30)
          + '&limit=' + 30
          + '&sort=' + backEndSortOperator + this.sortBy
      }).then( response => {
        this.tableData = [];
        var users = response.data.data.results;
        console.log(users);
        this.tableData = users;
      }).catch( (error) => {
        console.log(error);
        // console.log(error.response);
        // this.$message.error(error.response.data.description);
      });
    }
  },
  data () {
    return {
      link: "",
      employeeData: null,
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
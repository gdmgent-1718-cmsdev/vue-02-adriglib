<template>
  <div class="hello container">
    <h1>{{ msg }}</h1>
    <hr>
    <form class="form-horizontal">
      <div class="form-group">
        <label class="control-label col-sm-2" for="email">Email:</label>
        <div class="col-sm-10">
          <input type="email" class="form-control" id="email" placeholder="Enter email">
        </div>
      </div>
      <div class="form-group">
        <label class="control-label col-sm-2" for="pwd">Password:</label>
        <div class="col-sm-10">
          <input type="password" class="form-control" id="pwd" placeholder="Enter password">
        </div>
      </div>
      <div class="form-group">
        <div class="col-sm-offset-2 col-sm-10">
          <button type="submit" class="btn btn-default">Submit</button>
        </div>
      </div>
    </form>

    <h4>Here we have a list of all students and their courses:</h4>
    <div v-for="student in allStudents">
      Name: {{student.field_first_name[0].value}} <br>
      <div v-for="course in student.field_courses">
        {{course}}
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'read-article',
  data () {
    return {
      msg: 'Login',
      allStudents: []
    }
  },
  created () {
    axios
      .get('http://cmsdev.localhost/students')
      .then(({data: students}) => {
        console.log(students)
        this.allStudents = students
      })
      .catch(({message: error}) => {
        console.info(error)
      })

      // .catch(({message: error}) => {
      //   console.info(error.message)
      // })
  }

//  methods () {
//    var headers = {
//      'Content-Type': 'application/hal+json',
//      'Accept': 'application/hal+json'
//    }
//    axios.post('http://cmsdev.localhost', data, headers)
//
//      .then((response) => {
//        dispatch({type: FOUND_USER, data: response.data[0]})
//      })
//      .catch((error) => {
//        dispatch({type: ERROR_FINDING_USER})
//      })
//  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
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
  color: #35495E;
}
</style>

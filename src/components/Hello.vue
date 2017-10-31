<template>
  <div class="hello container">
    <!--<h1>{{ msg }}</h1>-->
    <!--<hr>-->
    <!--<form class="form-horizontal">-->
      <!--<div class="form-group">-->
        <!--<label class="control-label col-sm-2" for="email">Email:</label>-->
        <!--<div class="col-sm-10">-->
          <!--<input type="email" class="form-control" id="email" placeholder="Enter email">-->
        <!--</div>-->
      <!--</div>-->
      <!--<div class="form-group">-->
        <!--<label class="control-label col-sm-2" for="pwd">Password:</label>-->
        <!--<div class="col-sm-10">-->
          <!--<input type="password" class="form-control" id="pwd" placeholder="Enter password">-->
        <!--</div>-->
      <!--</div>-->
      <!--<div class="form-group">-->
        <!--<div class="col-sm-offset-2 col-sm-10">-->
          <!--<button type="submit" class="btn btn-default">Submit</button>-->
        <!--</div>-->
      <!--</div>-->
    <!--</form>-->

    <h4>Here we have a list of all students and their courses:</h4>
    <div v-for="student in allStudents">
      Name: {{student.field_first_name[0].value}} <br>
      <!--<div v-for="course in student.field_courses">-->
        <!--{{course}}-->
      <!--</div>-->
    </div>

    <div>
      <h4>Here you can add a new student to the database:</h4>
      <input placeholder="Add a new student" v-model="addStudentField" @keyup.enter="addStudent" @focus="clearSuccessFeedback"><br>
      <span>{{ successFeedback }}</span>
      <ul v-if="errors && errors.length">
        <li v-for="error of errors">
          {{error.message}}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'student',
  data () {
    return {
      msg: 'Login',
      allStudents: [],
      studentName: '',
      addStudentField: '',
      errors: [],
      successFeedback: ''
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
  },
  methods: {
    addStudent () {
      axios.post(`http://cmsdev.localhost/entity/student`, {
        'field_first_name': {
          'value': this.addStudentField
        }
      },
        {
          auth: {
            username: 'admin',
            password: 'secret'
          },
          headers: {
            'X-CSRF-Token': 'BsGhinsLa2wzQv9tPzaPbmZb_qxPJAUasEK2XjipaOY',
            'Accept': 'application/json',
            'Content-Type': 'application/json'
          }
        })
        .then(response => {},
        this.addStudentField = '',
        this.successFeedback = 'Added a new student to the database.')
        .catch(e => {
          this.errors.push(e)
        })
    },
    clearSuccessFeedback () {
      this.successFeedback = ''
    }
  }
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


<template>
   <div class="container">
    <div class="card">
        <div class="card-header">
            <h4>Students
                <RouterLink to="/students/create"
                 class="btn btn-primary float-end">Add Student
                </RouterLink>
            </h4>
        </div>
        <div class="card-body">
            <table class="table table-boardered">
                <thead>
                    <tr>
                        <th>ID</th>
                        <th>Name</th>
                        <th>Course</th>
                        <th>Email</th>
                        <th>Phone</th>
                        <th>Created At</th>
                        <th>Action</th>
                    </tr>
                </thead>
                <tbody v-if="this.students.length > 0">
                    <tr v-for="(student, index) in this.students" :key="index">
                        <th>{{ student.id }}</th>
                        <th>{{ student.name }}</th>
                        <th>{{ student.course }}</th>
                        <th>{{ student.email }}</th>
                        <th>{{ student.phone }}</th>
                        <th>{{ student.created_at }}</th>
                        <RouterLink :to="{path: '/students/'+student.id+'/edit'}"
                            class="btn btn-success">
                            Edit
                        </RouterLink>
                        <button type="button" @click="deleteStudent(student.id)" class="btn btn-danger">
                            Delete
                        </button>
                    </tr>
                </tbody>
                <tbody v-else>
                    <tr>
                        <td colspan="7">Loading...</td>
                    </tr>
                </tbody>
            </table>

        </div>
    </div>
   </div>  
  </template>

  <script>
  import axios from 'axios';

  export default {
    name: 'students',
    data() {
        return{
            students: []
        }
    },
    mounted() {
        this.getStudents();
        // console.log('I am here')
    }, 
    methods: {
        getStudents(){
            axios.get('/data/students.json').then(res=> {
                this.students = res.data.students
                console.log(this.students)

            });
        },
        deleteStudent(studentId){
            
            if(confirm('Are you sure,you want to delete this data?')){
               // console.log(studentId);
               axios.delete('students/{id}/delete')
               .then(res => {
                alert(res.data.message);
                this.getStudents();

               })
            }
        }
    }
  }
</script>
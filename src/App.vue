<template>
  <div id="app">
    
    <employee-form 
      @add:employee="addEmployee"></employee-form>
    <employee-table :employees="employees"
      @delete:employee="deleteEmployee"
      @edit:employee="editEmployee"></employee-table>
  </div>
</template>

<script>
import axios from 'axios'
import EmployeeTable from './components/EmployeeTable'
import EmployeeForm from './components/EmployeeForm'

export default {
  name: 'App',
  components: {
    EmployeeTable,
    EmployeeForm
  },
  data() {
    return {
      employees: []
    }
  },
  methods: {
    addEmployee(employee) {
      // const lastId =
      //   this.employees.length > 0
      //     ? this.employees[this.employees.length - 1].id
      //     : 0;
      // const id = lastId + 1;

      // const newEmployee = { ...employee, id };

      axios.post('https://jsonplaceholder.typicode.com/users', employee)
        .then((response) => {
          console.log(response.data);
          this.employees = [...this.employees, response.data] 
        })
        .catch((error) => console.log(error));

      

    },

    deleteEmployee(id) {
      axios.delete(`https://jsonplaceholder.typicode.com/users/${id}`)
        .then(() => {
          this.employees = this.employees.filter(employee => employee.id !== id);

        })
        .catch(error => console.log(error));
    },

    editEmployee(id, updatedEmployee) {

      axios.put(`https://jsonplaceholder.typicode.com/users/${id}`, updatedEmployee)
        .then(() => 
          this.employees = this.employees.map(employee => 
          employee.id === id ? updatedEmployee : employee
      ))
        .catch(error => console.log(error));

      
    },

    getEmployees() {
      axios.get('https://jsonplaceholder.typicode.com/users')
        .then((response) => this.employees = response.data)
        .catch((error) => console.log(error));
    }
    

  },
  mounted() {
    this.getEmployees()
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

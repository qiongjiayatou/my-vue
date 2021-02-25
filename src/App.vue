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
      employees: [
        {
          id: 1,
          name: 'Grace',
          email: 'grace@example.com'
        },
        {
          id: 2,
          name: 'Tomas',
          email: 'tom@example.com'
        },
        {
          id: 3,
          name: 'Angela',
          email: 'angela@example.com'
        },
      ]
    }
  },
  methods: {
    addEmployee(employee) {
      const lastId =
        this.employees.length > 0
          ? this.employees[this.employees.length - 1].id
          : 0;
      const id = lastId + 1;
      const newEmployee = { ...employee, id };

      this.employees = [...this.employees, newEmployee];

    },
    deleteEmployee(id) {
      this.employees = this.employees.filter(employee => employee.id !== id);
    },
    editEmployee(id, updatedEmployee) {
      this.employees = this.employees.map(employee => 
        employee.id === id ? updatedEmployee : employee
      )
    }
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

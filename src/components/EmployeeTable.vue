<template>
    <div class="container d-flex justify-content-center">
        <p v-if="employees.length < 1">Empty table</p>
        <table class="table" v-else>
            <thead>
                <tr>
                    <th>Name</th>
                    <th>Email</th>
                    <th></th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="employee in employees" :key="employee.id">
                    <td v-if="editing === employee.id">
                        <input type="text" v-model="employee.name" class="form-control">
                    </td>
                    <td v-else>{{ employee.name }}</td>
                    <td v-if="editing === employee.id">
                        <input type="text" v-model="employee.email" class="form-control">
                    </td>
                    <td v-else>{{ employee.email }}</td>
                    <td v-if="editing === employee.id">
                        <button @click="editEmployee(employee)" class="btn btn-primary mr-3">Save</button>
                        <button @click="cancelEdit(employee)" class="btn btn-secondary">Cancel</button>
                    </td>
                    <td v-else>
                        <button @click="editMode(employee)" class="btn btn-warning mr-3">Edit</button>
                        <button @click="$emit('delete:employee', employee.id)" class="btn btn-danger">Delete</button>
                    </td>
                </tr>
            </tbody>
            
        </table>
    </div>
</template>

<script>
export default {
    name: 'employee-table',
    props: {
        employees: Array
    },
    data() {
        return {
            editing: null,
            cachedEmployee: null
        }
    },
    methods: {
        editMode(employee) {
            this.cachedEmployee = Object.assign({}, employee)
            this.editing = employee.id;
        },

        editEmployee(employee) {
            if (employee.name === '' || employee.email === '') return 
            this.$emit('edit:employee', employee.id, employee)
            this.editing = null
        },

        cancelEdit(employee) {
            Object.assign(employee, this.cachedEmployee)
            this.editing = null;
        }


    }
    
    
    
}
</script>



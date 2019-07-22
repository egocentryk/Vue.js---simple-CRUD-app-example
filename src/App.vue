<template>
    <div id="app" class="container">
        <img class="logo" alt="Vue logo" src="./assets/logo.png" />

        <h1>Employees</h1>

        <employee-form @add:employee="addEmployee" />
        <employee-list 
            :employees="employees" 
            @delete:employee="deleteEmployee" 
            @edit:employee="editEmployee" 
        />
    </div>
</template>

<script>
    import EmployeeForm from '@/components/EmployeeForm.vue'
    import EmployeeList from '@/components/EmployeeList.vue'

    export default {
        name: 'app',
        components: {
            EmployeeForm,
            EmployeeList
        },

        data() {
            return {
                employees: []
            }
        },

        mounted() {
            this.getEmployees()
        },

        methods: {
            async getEmployees() {
                try {
                    const response = await fetch('https://jsonplaceholder.typicode.com/users')
                    const data = await response.json()
                    this.employees = data
                } catch (error) {
                    // eslint-disable-next-line
                    console.error(error)
                }
            },

            async addEmployee(employee) {
                try {
                    const response = await fetch('https://jsonplaceholder.typicode.com/users', {
                        method: 'POST',
                        body: JSON.stringify(employee),
                        headers: { "Content-type": "application/json; charset=UTF-8" }
                    })
                    const data = await response.json()
                    this.employees = [...this.employees, data]
                } catch (error) {
                    // eslint-disable-next-line
                    console.error(error)
                }
            },

            async editEmployee(id, updatedEmployee) {
                try {
                    const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
                        method: 'PUT',
                        body: JSON.stringify(updatedEmployee),
                        headers: { "Content-type": "application/json; charset=UTF-8" }
                    })
                    const data = await response.json()
                    this.employees = this.employees.map(employee => employee.id === id ? data : employee)
                } catch (error) {
                    // eslint-disable-next-line
                    console.error(error)
                }
            },

            async deleteEmployee(id) {
                try {
                    await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
                        method: 'DELETE'
                    })
                    this.employees = this.employees.filter(employee => employee.id !== id)
                } catch (error) {
                    // eslint-disable-next-line
                    console.error(error)
                }
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

    button {
        background: #009435;
        border: 1px solid #009435;
    }

    button:hover,
    button:active,
    button:focus {
        background: #32a95d;
        border: 1px solid #32a95d;
    }

    .container {
        max-width: 710px;
    }

    .logo {
        max-width: 50px;
    }
</style>

<template>
  <div id="employee-table">
    <p v-if="employees.length < 1" class="empty-table">No employees</p>
    <table v-else class="table table-striped">
      <thead>
        <tr>
          <th>Employee name</th>
          <th>Employee email</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="employee in employees" :key="employee.id">
          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.name" />
          </td>
          <td v-else>{{employee.name}}</td>
          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.email" />
          </td>
          <td v-else>{{employee.email}}</td>
          <td v-if="editing === employee.id">
            <button @click="editEmployee(employee)">Save</button>
            <!-- <button class="muted-button" @click="editing = null">Cancel</button> -->
            <button class="muted-button" @click="cancelEdit(employee)">Cancel</button>
          </td>
          <td v-else>
            <!-- <button @click="editMode(employee.id)">Edit</button> -->
            <button @click="editMode(employee)">Edit</button>
            <button @click="$emit('delete:employee', employee.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
  export default {
    name: 'EmployeeTable',
    props: {
      employees: Array,
    },
    data() {
      return {
        editing: null,
      }
    },
    methods: {
      // editMode(id) {
      //   this.editing = id
      // },
      editMode(employee) {
        this.cachedEmployee = Object.assign({}, employee)
        this.editing = employee.id
      },
      cancelEdit(employee) {
        Object.assign(employee, this.cachedEmployee)
        this.editing = null;
      },
      editEmployee(employee) {
        if (employee.name === '' || employee.email === '') return
        this.$emit('edit:employee', employee.id, employee)
        this.editing = null
      }
    }
  }
</script>

<style scoped>
  button {
    background: #009435;
    border: 1px solid #009435;
    margin: 0 0.5rem 0 0;
    color: #ffffff;
  } 

  button:hover {
    background: #00c746;
    border: 1px solid #00c746;
    color: #ffffff;
  } 
  
  input {
    margin: 0;
  } 
   
  .empty-table {
    text-align: center;
  } 
</style>
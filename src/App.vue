<template>
  <div>
    <h2>Employee Data [Post]</h2>
    <form action="post" name="add" id="add">
      <div class="form-input">
        <label>Name:</label>
        <el-input
          type="text"
          v-model="name"
          style="width: 240px"
          placeholder="Juan dela Cruz"
          required
        />
      </div>
      <div class="form-input">
        <label>Email:</label>
        <el-input
          type="email"
          v-model="email"
          style="width: 240px"
          placeholder="juan@delacruz.com"
          required
        />
      </div>
      <div class="form-input">
        <label>Position:</label>
        <el-input
          type="text"
          v-model="position"
          style="width: 240px"
          placeholder="Intern"
          required
        />
      </div>
      <div class="form-input">
        <label>Salary:</label>
        <el-input
          type="number"
          v-model="salary"
          style="width: 240px"
          placeholder="Basic Salary"
          required
        />
      </div>
      <div class="form-input">
        <label>SSS Number:</label>
        <el-input
          type="number"
          v-model="sssNumber"
          style="width: 240px"
          placeholder="000000000"
          required
        />
      </div>
      <div class="form-input">
        <label>Pag-Ibig Number:</label>
        <el-input
          type="number"
          v-model="pagIbigNumber"
          style="width: 240px"
          placeholder="000000000"
          required
        />
      </div>
      <el-button type="primary" @click="addEmployee" plain style="margin-left: 10px"
        >Add employee</el-button
      >
    </form>
  </div>

  <div style="margin-top: 20px">
    <h2>Employees Data [Get & Delete]</h2>
    <el-table :data="employees" border style="width: 100%; padding: 20px">
      <el-table-column prop="name" label="Name" width="180" />
      <el-table-column prop="email" label="Email" width="180" />
      <el-table-column prop="position" label="Position" width="180" />
      <el-table-column prop="salary" label="Salary" width="180" />
      <el-table-column prop="sssNumber" label="SSS Number" width="180" />
      <el-table-column prop="pagIbigNumber" label="Pag-Ibig Number" width="180" />
      <el-table-column label="Action" width="120">
        <template #default="scope">
          <el-button type="danger" size="small" @click="deleteEmployee(scope.row.id)">
            Delete
          </el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>

  <RouterView></RouterView>
</template>

<script>
import { defineComponent } from 'vue'

import axios from 'axios'

// const ApiCall = 'https://swapi.dev/api/'
export default defineComponent({
  data() {
    return {
      likeCounter: 0,
      employees: [],
      name: '',
      position: '',
      email: '',
      salary: '',
      sssNumber: '',
      pagIbigNumber: '',
    }
  },
  methods: {
    addLike() {
      this.likeCounter++
    },
    getEmployees() {
      axios.get(`https://localhost:7043/employee`).then((response) => {
        this.employees = response.data
      })
    },
    addEmployee() {
      axios.post(`https://localhost:7043/employee`, {
        name: this.name,
        email: this.email,
        position: this.position,
        salary: this.salary,
        sssNumber: this.sssNumber,
        pagIbigNumber: this.pagIbigNumber,
      })
      alert('Employeed succesfully added')
      location.reload()
    },
    deleteEmployee(employeeId) {
      axios
        .delete(`https://localhost:7043/employee/${employeeId}`)
        .then(() => {
          console.log(`Delete post with id ${employeeId}`)
          alert('Employee successfully deleted')
          location.reload()
        })
        .catch((error) => {
          console.error(error)
        })
    },
  },
  components: {
    // SampolCompownent,
  },
  created() {
    this.getEmployees()
  },
  mounted() {
    // alert('Vue loaded')
    console.log('test')
    console.log(this.employees)
  },
})
</script>

<style>
.form-input {
  display: flex;
  margin: 10px;
}
.form-input label {
  min-width: 150px;
}
</style>

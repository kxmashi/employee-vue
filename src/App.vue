<template>
  <div class="common-layout">
    <el-container>
      <el-header class="sk-c">
        <h1>SkanLog</h1>
      </el-header>

      <el-main>
        <el-row :gutter="20">
          <!-- form column -->
          <el-col :span="6">
            <div class="grid-content ep-bg-purple-light">
              <div class="add-form">
                <h2>Employee Data [Post]</h2>
                <el-form
                  ref="formRef"
                  :model="employeeData"
                  style="max-width: 600px"
                  class="demo-ruleForm"
                  action="post"
                  name="add"
                  id="add"
                  @submit.prevent="submitForm"
                >
                  <el-form-item
                    label="Name"
                    prop="name"
                    :rules="[{ required: true, message: 'Name is required' }]"
                  >
                    <el-input v-model="employeeData.name" type="text" autocomplete="off" />
                  </el-form-item>

                  <el-form-item
                    label="Email"
                    prop="email"
                    :rules="[
                      { required: true, message: 'Email is required' },
                      { type: 'email', message: 'Email must be a valid email' },
                    ]"
                  >
                    <el-input v-model="employeeData.email" type="text" autocomplete="off" />
                  </el-form-item>

                  <el-form-item
                    label="Position"
                    prop="position"
                    :rules="[{ required: true, message: 'Position is required' }]"
                  >
                    <el-input v-model="employeeData.position" type="text" autocomplete="off" />
                  </el-form-item>

                  <el-form-item
                    label="Salary"
                    prop="salary"
                    :rules="[
                      { required: true, message: 'Salary is required' },
                      { type: 'number', message: 'Salary must be a valid number' },
                    ]"
                  >
                    <el-input v-model="employeeData.salary" type="number" autocomplete="off" />
                  </el-form-item>

                  <el-form-item
                    label="SSS Number"
                    prop="sssNumber"
                    :rules="[
                      { required: true, message: 'SSS Number is required' },
                      { type: 'number', message: 'SSS Number must be a valid number' },
                    ]"
                  >
                    <el-input
                      v-model="employeeData.sssNumber"
                      type="number"
                      autocomplete="off"
                      :max-length="10"
                    />
                  </el-form-item>

                  <el-form-item
                    label="Pag-Ibig Number"
                    prop="pagIbigNumber"
                    :rules="[
                      { required: true, message: 'Pag-Ibig Number is required' },
                      { type: 'number', message: 'Pag-Ibig Number must be a valid number' },
                    ]"
                  >
                    <el-input
                      v-model="employeeData.pagIbigNumber"
                      type="number"
                      autocomplete="off"
                      :max-length="10"
                    />
                  </el-form-item>

                  <el-form-item>
                    <el-button type="primary" native-type="submit">Submit</el-button>
                  </el-form-item>
                </el-form>
              </div>
            </div>
          </el-col>
          <!-- form column -->

          <el-col :span="18">
            <div class="grid-content ep-bg-purple">
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
            </div>
          </el-col>
        </el-row>

        <RouterView></RouterView>
      </el-main>

      <el-footer style="background-color: black; color: white"></el-footer>
    </el-container>
  </div>
</template>

<script>
import { defineComponent } from 'vue'
import axios from 'axios'
import { ElMessage } from 'element-plus'

// const ApiCall = 'https://swapi.dev/api/'
export default defineComponent({
  data() {
    return {
      likeCounter: 0,
      employees: [],
      employeeData: {
        name: '',
        position: '',
        email: '',
        salary: '',
        sssNumber: '',
        pagIbigNumber: '',
      },
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
    deleteEmployee(employeeId) {
      axios
        .delete(`https://localhost:7043/employee/${employeeId}`)
        .then(() => {
          ElMessage({
            message: 'Employee added deleted.',
            type: 'success',
            plain: true,
          })
          location.reload()
        })
        .catch((error) => {
          console.error(error)
        })
    },
    submitForm() {
      this.$refs.formRef.validate((valid) => {
        if (valid) {
          console.log('Form submitted!')
          axios
            .post(`https://localhost:7043/employee`, this.employeeData)
            .then(() => {
              ElMessage({
                message: 'Employee added successfully.',
                type: 'success',
                plain: true,
              })
              location.reload()
            })
            .catch((error) => {
              console.error('Error adding employee:', error)
              ElMessage({
                message: 'Error adding employee.',
                type: 'warning',
                plain: true,
              })
            })
        } else {
          ElMessage({
            message: 'All required inputs must be valid and not be empty.',
            type: 'warning',
            plain: true,
          })
          return false
        }
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
.add-form {
  border: 1px solid #ccc;
  padding: 20px;
  border-radius: 10px;
}

.el-col {
  border-radius: 4px;
}

.grid-content {
  border-radius: 4px;
  min-height: 36px;
}

.el-form-item__label {
  width: 150px;
  justify-content: flex-start !important;
}
</style>

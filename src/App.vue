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
                  ref="addFormRef"
                  :model="employeeData"
                  style="max-width: 600px"
                  class="demo-ruleForm"
                  action="post"
                  name="add"
                  id="add"
                  @submit.prevent="createEmployee"
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
                    :rules="[{ required: true, message: 'Salary is required' }]"
                  >
                    <el-input v-model="employeeData.salary" type="number" autocomplete="off" />
                  </el-form-item>

                  <el-form-item
                    label="SSS Number"
                    prop="sssNumber"
                    :rules="[{ required: true, message: 'SSS Number is required' }]"
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
                    :rules="[{ required: true, message: 'Pag-Ibig Number is required' }]"
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

          <!-- table column -->
          <el-col :span="18">
            <div class="grid-content ep-bg-purple">
              <div style="margin-top: 20px">
                <h2>Employees Data [Get , Update, & Delete]</h2>
                <label>Search: </label>
                <el-input
                  v-model="search"
                  style="width: 240px"
                  placeholder="Please Input"
                  @input="updateModel(search)"
                />
                <el-table :data="employees" border style="width: 100%; padding: 20px">
                  <el-table-column prop="name" label="Name" width="180" />
                  <el-table-column prop="email" label="Email" width="180" />
                  <el-table-column prop="position" label="Position" width="180" />
                  <el-table-column prop="salary" label="Salary" width="180" />
                  <el-table-column prop="sssNumber" label="SSS Number" width="180" />
                  <el-table-column prop="pagIbigNumber" label="Pag-Ibig Number" width="180" />
                  <el-table-column label="Action" width="150">
                    <template #default="scope">
                      <el-button
                        type="danger"
                        size="small"
                        @click="openDeleteConfirm(scope.row.id)"
                      >
                        Delete
                      </el-button>
                      <el-button type="primary" size="small" @click="openEditModal(scope.row.id)">
                        Edit
                      </el-button>
                    </template>
                  </el-table-column>
                </el-table>
              </div>
            </div>
          </el-col>
        </el-row>
        <!-- table column -->

        <RouterView></RouterView>
      </el-main>

      <el-footer style="background-color: black; color: white"></el-footer>
    </el-container>
  </div>

  <!-- edit modal -->
  <el-dialog
    v-model="editDialogVisible"
    modal-class="overide-animation"
    :before-close="
      (doneFn) => {
        console.log('before-close'), doneFn()
      }
    "
  >
    <h2>Edit employee detail</h2>
    <el-form
      ref="editFormRef"
      :model="editEmployeeData"
      style="max-width: 600px; padding: 20px"
      class="demo-ruleForm"
      action="post"
      name="add"
      id="add"
      @submit.prevent="editEmployee(employeeId)"
    >
      <el-form-item
        label="Name"
        prop="name"
        :rules="[{ required: true, message: 'Name is required' }]"
      >
        <el-input v-model="editEmployeeData.name" type="text" autocomplete="off" />
      </el-form-item>

      <el-form-item
        label="Email"
        prop="email"
        :rules="[
          { required: true, message: 'Email is required' },
          { type: 'email', message: 'Email must be a valid email' },
        ]"
      >
        <el-input v-model="editEmployeeData.email" type="text" autocomplete="off" />
      </el-form-item>

      <el-form-item
        label="Position"
        prop="position"
        :rules="[{ required: true, message: 'Position is required' }]"
      >
        <el-input v-model="editEmployeeData.position" type="text" autocomplete="off" />
      </el-form-item>

      <el-form-item
        label="Salary"
        prop="salary"
        :rules="[{ required: true, message: 'Salary is required' }]"
      >
        <el-input v-model="editEmployeeData.salary" type="number" autocomplete="off" />
      </el-form-item>

      <el-form-item
        label="SSS Number"
        prop="sssNumber"
        :rules="[{ required: true, message: 'SSS Number is required' }]"
      >
        <el-input v-model="editEmployeeData.sssNumber" type="number" autocomplete="off" />
      </el-form-item>

      <el-form-item
        label="Pag-Ibig Number"
        prop="pagIbigNumber"
        :rules="[{ required: true, message: 'Pag-Ibig Number is required' }]"
      >
        <el-input v-model="editEmployeeData.pagIbigNumber" type="number" autocomplete="off" />
      </el-form-item>
    </el-form>

    <template #footer>
      <div class="dialog-footer">
        <el-button @click="editDialogVisible = false">Cancel</el-button>
        <el-button type="primary" @click="editEmployee(employeeId)"> Submit </el-button>
      </div>
    </template>
  </el-dialog>
  <!-- edit modal -->

  <!-- confirm delete pop -->
  <el-dialog v-model="deleteConfirmDialog" title="Warning" width="500" center>
    <span> Are you sure you want to delete this employee? </span>
    <template #footer>
      <div class="dialog-footer">
        <el-button @click="deleteConfirmDialog = false">Cancel</el-button>
        <el-button type="primary" @click="deleteEmployee(employeeId)"> Confirm </el-button>
      </div>
    </template>
  </el-dialog>
  <!-- confirm delete pop -->
</template>

<script>
import { defineComponent } from 'vue'
import axios from 'axios'
import { ElMessage } from 'element-plus'
import _debounce from 'lodash/debounce'

// const ApiCall = 'https://swapi.dev/api/'
export default defineComponent({
  data() {
    return {
      likeCounter: 0,
      employees: [],
      employeeId: '',
      employeeData: {
        name: '',
        position: '',
        email: '',
        salary: '',
        sssNumber: '',
        pagIbigNumber: '',
      },
      editEmployeeData: {
        name: '',
        position: '',
        email: '',
        salary: '',
        sssNumber: '',
        pagIbigNumber: '',
      },
      search: '',
      editDialogVisible: false,
      deleteConfirmDialog: false,
    }
  },
  props: {},
  methods: {
    addLike() {
      this.likeCounter++
    },
    getEmployees(search) {
      if (!search) {
        axios.get(`https://localhost:7043/employee`).then((response) => {
          this.employees = response.data
        })
      } else {
        axios
          .get(`https://localhost:7043/employee/filter` + '?search=' + search)
          .then((response) => {
            this.employees = response.data
          })
      }
    },
    updateModel: _debounce(function (newVal) {
      this.getEmployees(newVal)
    }, 500),
    openDeleteConfirm(employeeId) {
      if (employeeId) {
        this.deleteConfirmDialog = true
        this.employeeId = employeeId
      }
    },
    deleteEmployee(employeeId) {
      axios
        .delete(`https://localhost:7043/employee/${employeeId}`)
        .then(() => {
          ElMessage({
            message: 'Employee deleted successfullt.',
            type: 'success',
            plain: true,
          })
          this.deleteConfirmDialog = false
          this.getEmployees()
        })
        .catch((error) => {
          console.error(error)
        })
    },
    openEditModal(id) {
      if (id) {
        this.editDialogVisible = true
        axios.get(`https://localhost:7043/employee/${id}`).then((response) => {
          this.editEmployeeData = response.data
          this.employeeId = id
        })
      }
    },
    editEmployee(employeeId) {
      this.$refs.editFormRef.validate((valid) => {
        if (valid) {
          axios
            .put(`https://localhost:7043/employee` + `/${employeeId}`, this.editEmployeeData)
            .then(() => {
              ElMessage({
                message: 'Employee edited successfully.',
                type: 'success',
                plain: true,
              })
              this.getEmployees()
              this.editDialogVisible = false
            })
            .catch((error) => {
              console.error('Error adding employee:', error.response.data)
              ElMessage({
                message: 'Error adding employee: "' + error.response.data.message + '"',
                type: 'error',
                plain: true,
              })
            })
        } else {
          ElMessage({
            message: 'All required inputs must be valid and not be empty.',
            type: 'error',
            plain: true,
          })
          return false
        }
      })
    },
    createEmployee() {
      this.$refs.addFormRef.validate((valid) => {
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
              this.getEmployees()
              this.employeeData = new FormData()
            })
            .catch((error) => {
              console.error('Error adding employee:', error.response.data.message)
              ElMessage({
                message: 'Error adding employee: "' + error.response.data.message + '"',
                type: 'error',
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
    // this.getEmployees()
  },
  mounted() {
    // alert('Vue loaded')
    this.getEmployees()
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

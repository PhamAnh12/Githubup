<template>
  <div>
    <h2>Danh sách nhân viên</h2>
    <b-button variant="primary" @click="showAddModal" class="mb-3">Thêm nhân viên</b-button>
    
    <b-table striped hover :items="employees" :fields="fields">
      <template #cell(actions)="row">
        <b-button size="sm" @click="editEmployee(row.item)" class="mr-2">Sửa</b-button>
        <b-button size="sm" variant="danger" @click="deleteEmployee(row.item.id)">Xóa</b-button>
      </template>
    </b-table>

    <b-modal v-model="showModal" :title="isEdit ? 'Sửa nhân viên' : 'Thêm nhân viên'" @hidden="resetModal">
      <EmployeeForm 
        :employee="selectedEmployee" 
        @submit="handleSubmit" 
        @cancel="showModal = false" 
      />
    </b-modal>
  </div>
</template>

<script>
import axios from 'axios'
import EmployeeForm from './EmployeeForm.vue'

export default {
  components: { EmployeeForm },
  data() {
    return {
      employees: [],
      fields: [
        { key: 'id', label: 'ID' },
        { key: 'name', label: 'Họ tên' },
        { key: 'email', label: 'Email' },
        { key: 'phone', label: 'Điện thoại' },
        { key: 'position', label: 'Chức vụ' },
        { key: 'department', label: 'Phòng ban' },
        { key: 'actions', label: 'Thao tác' }
      ],
      showModal: false,
      isEdit: false,
      selectedEmployee: null
    }
  },
  created() {
    this.fetchEmployees()
  },
  methods: {
    async fetchEmployees() {
      try {
        const response = await axios.get('http://localhost:5000/api/employees')
        this.employees = response.data
      } catch (error) {
        console.error(error)
      }
    },
    showAddModal() {
      this.isEdit = false
      this.selectedEmployee = null
      this.showModal = true
    },
    editEmployee(employee) {
      this.isEdit = true
      this.selectedEmployee = { ...employee }
      this.showModal = true
    },
    async deleteEmployee(id) {
      if (confirm('Bạn có chắc muốn xóa nhân viên này?')) {
        try {
          await axios.delete(`http://localhost:5000/api/employees/${id}`)
          this.fetchEmployees()
        } catch (error) {
          console.error(error)
        }
      }
    },
    async handleSubmit(employee) {
      try {
        if (this.isEdit) {
          await axios.put(`http://localhost:5000/api/employees/${employee.id}`, employee)
        } else {
          await axios.post('http://localhost:5000/api/employees', employee)
        }
        this.showModal = false
        this.fetchEmployees()
      } catch (error) {
        console.error(error)
      }
    },
    resetModal() {
      this.selectedEmployee = null
    }
  }
}
</script>
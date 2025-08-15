<template>
  <div>
    <b-form @submit.prevent="handleSubmit">
      <b-form-group label="Họ tên:" label-for="name">
        <b-form-input id="name" v-model="form.name" required></b-form-input>
      </b-form-group>

      <b-form-group label="Email:" label-for="email">
        <b-form-input id="email" v-model="form.email" type="email" required></b-form-input>
      </b-form-group>

      <b-form-group label="Điện thoại:" label-for="phone">
        <b-form-input id="phone" v-model="form.phone"></b-form-input>
      </b-form-group>

      <b-form-group label="Chức vụ:" label-for="position">
        <b-form-input id="position" v-model="form.position"></b-form-input>
      </b-form-group>

      <b-form-group label="Phòng ban:" label-for="department">
        <b-form-input id="department" v-model="form.department"></b-form-input>
      </b-form-group>

      <div class="mt-4">
        <b-button type="submit" variant="primary">Lưu</b-button>
        <b-button type="button" variant="secondary" @click="$emit('cancel')" class="ml-2">Hủy</b-button>
      </div>
    </b-form>
  </div>
</template>

<script>
export default {
  props: {
    employee: {
      type: Object,
      default: null
    }
  },
  data() {
    return {
      form: {
        id: null,
        name: '',
        email: '',
        phone: '',
        position: '',
        department: ''
      }
    }
  },
  watch: {
    employee: {
      immediate: true,
      handler(val) {
        if (val) {
          this.form = { ...val }
        } else {
          this.resetForm()
        }
      }
    }
  },
  methods: {
    resetForm() {
      this.form = {
        id: null,
        name: '',
        email: '',
        phone: '',
        position: '',
        department: ''
      }
    },
    handleSubmit() {
      this.$emit('submit', this.form)
    }
  }
}
</script>
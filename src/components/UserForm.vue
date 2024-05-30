<template>
  <div class="modal" v-if="dialog">
    <div class="modal-content">
      <span class="close" @click="close">&times;</span>
      <h2>{{ user ? 'Edit User' : 'Add User' }}</h2>
      <form @submit.prevent="save" class="user-form">
        <div class="form-group">
          <label for="name">Name</label>
          <input type="text" v-model="formData.name" required/>
        </div>
        <div class="form-group">
          <label for="email">Email</label>
          <input type="text" v-model="formData.login" required/>
        </div>
        <div class="form-group">
          <label for="password">Password</label>
          <input type="password" name="password" v-model="formData.password" required/>
        </div>
        <div class="form-group">
          <label for="userType">User Type</label>
          <UserTypeSelect v-model="formData.user_type_id" :user-types="userTypes"/>
        </div>
        <div class="button-group">
          <button type="submit" class="primary">Save</button>
          <button type="button" @click="close" class="danger">Cancel</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import UserTypeSelect from './UserTypeSelect.vue';

export default {
  name: 'UserForm',
  components: {UserTypeSelect},
  props: {
    user: Object,
    userTypes: Array,
  },
  data() {
    return {
      dialog: true,
      formData: {
        name: this.user ? this.user.name : '',
        login: this.user ? this.user.login : '',
        password: this.user ? this.user.password : '',
        user_type_id: this.user ? this.user.user_type_id : null,
        sm_user_id: this.user ? this.user.sm_user_id : null,
      },
    };
  },
  methods: {
    close() {
      this.$emit('close');
    },
    save() {
      this.formData.sm_user_type_id = this.formData.user_type_id;
      this.formData.new_user_type_id = this.formData.user_type_id;
      this.formData.new_name = this.formData.name;
      this.formData.new_password = this.formData.password;
      this.formData.new_login = this.formData.login;
      this.formData.new_sm_user_type_id = this.formData.sm_user_id;
      this.$emit('save', this.formData);
    },
  },
};
</script>


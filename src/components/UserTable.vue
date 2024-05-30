<template>
  <div>
    <button @click="openAddForm" class="primary">Добавить</button>
    <table>
      <thead>
      <tr>
        <th>ID</th>
        <th>Имя</th>
        <th>Логин</th>
        <th>Пароль</th>
        <th>Тип</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="user in users" :key="user.sm_user_id">
        <td>{{ user.sm_user_id }}</td>
        <td>{{ user.name }}</td>
        <td>{{ user.login }}</td>
        <td>{{ user.password }}</td>
        <td>{{ getUserTypeName(user.user_type_id) }}</td>
        <td>
          <button @click="openEditForm(user)" class="primary">Изменить</button>
          <button @click="deleteUser(user.sm_user_id)" class="danger">Удалить</button>
        </td>
      </tr>
      </tbody>
    </table>
    <UserForm
        v-if="showForm"
        :user="selectedUser"
        :user-types="userTypes"
        @close="closeForm"
        @save="saveUser"
    />
  </div>
</template>

<script>
import axios from 'axios';
import UserForm from './UserForm.vue';
export default {
  name: 'UserTable',
  components: {
    UserForm,
  },
  data() {
    return {
      users: [],
      userTypes: [],
      showForm: false,
      selectedUser: null,
    };
  },
  methods: {
    fetchUsers() {
      axios.get('http://91.147.91.163:8888/api/crud/users')
          .then(response => {
            if (response.data.success) {
              this.users = response.data.content.db_rows;
            } else {
              console.error('Error fetching users:', response.data.message);
            }
          })
          .catch(error => {
            console.error('Error fetching users:', error);
          });
    },
    fetchUserTypes() {
      axios.get('http://91.147.91.163:8888/api/crud/user-types')
          .then(response => {
            if (response.data.success) {
              this.userTypes = response.data.content.db_rows;
            } else {
              console.error('Ошбка', response.data.message);
            }
          })
          .catch(error => {
            console.error('Ошибка', error);
          });
    },

    getUserTypeName(userTypeId) {
      if (userTypeId) {
        const userType = this.userTypes.find(type => type.sm_user_type_id === userTypeId);
        return userType ? userType.name : 'Пусто';
      }
    },
    deleteUser(userId) {
      axios.delete(`http://91.147.91.163:8888/api/crud/users/${userId}`)
          .then(() => {
            this.fetchUsers();
          })
          .catch(error => {
            console.error('Ошибка', error);
          });
    },
    openAddForm() {
      this.selectedUser = null;
      this.showForm = true;
    },
    openEditForm(user) {
      this.selectedUser = user;
      this.showForm = true;
    },
    closeForm() {
      this.showForm = false;
      this.fetchUsers();
    },
    saveUser(user) {
      const action = user.sm_user_id ? 'put' : 'post';
      const url = user.sm_user_id ? `http://91.147.91.163:8888/api/crud/users/${user.sm_user_id}` : 'http://91.147.91.163:8888/api/crud/users';
      axios[action](url, user)
          .then(() => {
            this.fetchUsers();
            this.closeForm();
          })
          .catch(error => {
            console.error('Error saving user:', error);
          });
    },
  },
  created() {
    this.fetchUsers();
    this.fetchUserTypes();
  },
};
</script>

<style scoped>
table {
  width: 100%;
  border-collapse: collapse;
  margin: 20px 0;
}

table, th, td {
  border: 1px solid black;
}

th, td {
  padding: 10px;
  text-align: left;
}

button {
  margin-right: 5px;
}
</style>
  
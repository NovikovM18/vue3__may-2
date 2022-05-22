<template>
  <UsersList :users="usersFromServer"/>
  <Form @addUser="addUser"/>  
</template>

<script>
import Form from '@/components/Form.vue'
import UsersList from '@/components/UsersList.vue'

export default {
  name: 'App',
  components: {
    Form,
    UsersList
  },
  data() {
    return {
      usersFromServer: [],
      token: ''
    }
  },
  methods: {
    async fetchUsers() {
      try {
        const response = await fetch('https://frontend-test-assignment-api.abz.agency/api/v1/users?page=1&count=3');
        const data = await response.json();
        this.usersFromServer = data.users;
      } catch(error) {
        console.log(error);
      }
    },

    async fetchToken() {
      try {
        const response = await fetch('https://frontend-test-assignment-api.abz.agency/api/v1/token');
        const data = await response.json();
        this.token = data.token;
      } catch(error) {
        console.log(error);
      }
    },

    async addUser(newUser) {
      try {
        const formData = new FormData();
        formData.append('position_id', newUser.position_id); 
        formData.append('name', newUser.name); 
        formData.append('email', newUser.email); 
        formData.append('phone', newUser.phone); 
        formData.append('photo', newUser.photo);
        const response = await fetch('https://frontend-test-assignment-api.abz.agency/api/v1/users', { 
          method: 'POST', 
          body: formData, 
          headers: {'Token': this.token}, 
        })
        const data = await response.json();
          console.log(data);
        if (data.success === false) {
          alert(data.message);
        } else if (data.success === true) {
          this.fetchUsers();
        }
      } catch(error) {
        console.log(error);
      }
    }
  },
  mounted() {
    this.fetchUsers();
    this.fetchToken();
  }
}
</script>


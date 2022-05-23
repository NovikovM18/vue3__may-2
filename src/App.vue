<template>
  <v-app>
    <v-main>
        <Header />
        <Start />
        <UsersList :users="usersFromServer" :stopPagination="stopPagination" @showMore="showMore"/>
        <Form :successfull="successfull" @addUser="addUser"/> 
    </v-main>
  </v-app>
</template>


<script>
import Form from '@/components/Form.vue'
import UsersList from '@/components/UsersList.vue'
import Header from '@/components/Header.vue'
import Start from '@/components/Start.vue'

export default {
  name: 'App',
  components: {
    Form,
    UsersList,
    Header,
    Start
  },
  data() {
    return {
      loadingCount: 6,
      totalUsers: 7,
      stopPagination: false,
      usersFromServer: [],
      token: '',
      successfull: false
    }
  },
  methods: {
    async fetchUsers() {
      try {
        const res = await fetch(`https://frontend-test-assignment-api.abz.agency/api/v1/users`);
        const dataRes = await res.json();
        this.totalUsers = dataRes.total_users;
        const response = await fetch(`https://frontend-test-assignment-api.abz.agency/api/v1/users?page=1&count=${this.loadingCount}`);
        const data = await response.json();
        this.usersFromServer = data.users;
      } catch(error) {
        alert(error);
      }
    },

    async fetchToken() {
      try {
        const response = await fetch('https://frontend-test-assignment-api.abz.agency/api/v1/token');
        const data = await response.json();
        this.token = data.token;
      } catch(error) {
        alert(error);
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
        if (data.success === false) {
          alert(data.message);
        } else if (data.success === true) {
          this.successfull = true;
          this.fetchUsers();
        }
      } catch(error) {
        alert(error);
      }
    },
    showMore() {
      this.loadingCount += 6;
      if (this.loadingCount >= this.totalUsers) {
        this.stopPagination = true;
      }
      this.fetchUsers();
    }
  },
  mounted() {
    this.fetchUsers();
    this.fetchToken();
  }
}
</script>


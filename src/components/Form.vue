<template>
  <h1>Working with POST request</h1>
  <form 
    @submit.prevent="signUp"
    enctype="multipart/form-data" 
    class="form"
  >
    <input type="text" placeholder="name" v-model="newUser.name">
    <input type="email" placeholder="Email" v-model="newUser.email">
    <input type="phone" placeholder="Phone" v-model="newUser.phone">
      <div v-for="position in positions" :key="position.id">
        <input type="radio" :id="position.id" :value="position.id" :name="'position-selected'" v-model="newUser.position_id">
        <label :for="position.id">{{position.name}}</label>
      </div>
    <input
      type="file"
      ref="photo"
      @change="selectPhoto"
    >
    <button>Sign up</button>
  </form>
</template>

<script>
export default {
  emits: ['addUser'],
  data() {
    return {
      positions: [],
      newUser: {
        name: '',
        email: '',
        phone: '',
        position_id: '',
        photo:''
      },
    }
  },
  methods: {
    selectPhoto() {
      this.newUser.photo = this.$refs.photo.files[0];
    },
    
    async fetchPosotions() {
      try {
        const response = await fetch('https://frontend-test-assignment-api.abz.agency/api/v1/positions');
        const data = await response.json();
        this.positions = data.positions
      } catch(error) {
        console.log(error);
      }
    },
    
    signUp() {
      this.$emit('addUser', this.newUser);
      this.newUser = {
        name: '',
        email: '',
        phone: '',
        position_id: '',
        photo:''
      };
      this.$refs.photo.value = '';
    }
  },
  mounted() {
    this.fetchPosotions();
  }
}
</script>
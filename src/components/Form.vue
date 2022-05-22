<template>
  <h1>Working with POST request</h1>
  <form 
    @submit.prevent
    enctype="multipart/form-data" 
    class="form"
  > 
    <v-text-field :rules="[rules.required, rules.name]" label="Name" variant="outlined" v-model="newUser.name"></v-text-field>
    <v-text-field :rules="[rules.required, rules.email]" label="Email" variant="outlined" v-model="newUser.email"></v-text-field>
    <v-text-field :rules="[rules.required, rules.phone]" label="Phone" variant="outlined" v-model="newUser.phone"></v-text-field>
    <v-radio-group
      v-model="newUser.position_id"
      column
      v-for="position in positions"
      :key="position.id"
    >
      <v-radio
        :label="position.name"
        color="#00BDD3"
        :value="position.id"
      ></v-radio>
    </v-radio-group>
    <div class="file">
      <v-btn id="file__button" variant="outlined" type="button" @click="selectPhoto">Upload</v-btn>
      <v-file-input
        :rules="[rules.required, rules.photo]"
        accept="image/jpg, image/jpeg"
        ref="photo"
        label="Upload your photo"
        variant="outlined"
      ></v-file-input>
    </div>
    <v-btn variant="outlined" @click="signUp">Sign up</v-btn>
  </form>
</template>

<script>
export default {
  emits: ['addUser'],
  data() {
    return {
      rules: {
        required: value => !!value || 'Required',
        name: value => value.length >= 2 || 'Min 2 characters' && value.length <= 60 || 'Max 60 characters',
        phone: value => {
          const pattern = /^[\+]{0,1}380([0-9]{9})$/
          return pattern.test(value) || 'Invalid phone'
        },
        email: value => {
          const pattern = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
          return pattern.test(value) || 'Invalid e-mail'
        },
        photo: value => {
          return !value || !value.length || value[0].size < 5000000 || 'Photo size should be less than 5 MB!'
        }
      },
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
      // this.newUser = {
      //   name: '',
      //   email: '',
      //   phone: '',
      //   position_id: '',
      //   photo:''
      // };
      // this.$refs.photo.value = '';
    }
  },
  mounted() {
    this.fetchPosotions();
  }
}
</script>
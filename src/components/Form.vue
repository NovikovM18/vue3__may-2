<template>
  <div class="container">
    <h1 id="sign-up">Working with POST request</h1>
    <form
      @submit.prevent
      enctype="multipart/form-data" 
      class="form"
    > 
      <v-text-field :rules="[rules.required, rules.min, rules.max]" label="Name" variant="outlined" v-model="newUser.name"></v-text-field>
      <v-text-field :rules="[rules.required, rules.email]" label="Email" variant="outlined" v-model="newUser.email"></v-text-field>
      <v-text-field :rules="[rules.required, rules.phone]" hint="+38 (XXX) XXX - XX - XX" label="Phone" variant="outlined" v-model="newUser.phone"></v-text-field>
      <div>
        <p class="form__position-text">Select your position</p>
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
          >
          </v-radio>
        </v-radio-group>
      </div>
      <div class="file">
        <v-btn id="file__button" variant="outlined" type="button" @click="selectPhoto">Upload</v-btn>
        <v-file-input
          :rules="[rules.required, rules.photo]"
          accept="image/jpg, image/jpeg"
          ref="photo"
          label="Upload your photo"
          variant="outlined"
        >
        </v-file-input>
      </div>
      <v-btn class="button" variant="outlined" @click="signUp">Sign up</v-btn>
    </form>
    <div v-if="successfull" class="successfull">
      <h1>User successfully registered</h1>
        <div class="successfull__img">
          <v-img
            src="../assets/success.svg"
          >
          </v-img>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    successfull: {
      type: Boolean,
      required: true
    }
  },
  emits: ['addUser'],
  data() {
    return {
      rules: {
        required: value => !!value || 'Required',
        min: value => value.length >= 2 || 'Min 2 characters',
        max: value => value.length <= 60 || 'Max 60 characters',
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
      }
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
      this.$refs.photo.files = null;
    }
  },
  mounted() {
    this.fetchPosotions();
  }
}
</script>
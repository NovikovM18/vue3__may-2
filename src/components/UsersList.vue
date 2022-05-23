<template>
  <div class="container">
    <h1 id="users">Working with GET request</h1>
    <div class="users-list">
      <div v-for="user in users" :key="user.id">
        <UserCard :user="user"/>
      </div>
    </div>
    <v-btn v-if="!stopPagination" class="button list-button" variant="outlined" @click="showMore">Show more</v-btn>
  </div>
</template>

<script>
import UserCard from '@/components/UserCard.vue'

export default {
  emits: ['showMore'],
  components: {
    UserCard
  },
  props: {
    users: {
      type: Array,
      required: true
    },
    stopPagination: {
      type: Boolean,
      required: true
    }
  },
  methods: {
    showMore() {
      this.$emit('showMore');
    }
  }
}
</script>
<template>
  <div v-for="user in users" :key="user.id">
    <UserCard :user="user"/>
  </div>
</template>

<script>
import UserCard from '@/components/UserCard.vue'

export default {
  components: {
    UserCard
  },
  props: {
    users: {
      type: Array,
      required: true
    }
  }
}
</script>
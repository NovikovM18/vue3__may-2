<template>
  <img :src=user.photo alt="">
  <p>{{user.name}}</p>
  <p>{{user.position}}</p>
  <p>{{user.email}}</p>
  <p>{{user.phone}}</p>
</template>

<script>
export default {
  props: {
    user: {
      type: Object,
      required: true
    }
  }
}
</script>
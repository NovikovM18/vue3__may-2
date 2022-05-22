<template>
    <div class="card">
    <img :src=user.photo alt="photo">
    <p>{{user.name}}</p>
    <p>{{user.position}}</p>
    <p>{{user.email}}</p>
    <p>{{user.phone}}</p>
    </div>
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
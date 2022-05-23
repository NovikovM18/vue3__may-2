<template>
    <div class="card">
      <img :src=user.photo alt="photo" class="card__photo">
      <p class="card__name">{{user.name}}</p>
      <div class="card__description">
        <p class="card__position">{{user.position}}</p>
        <p class="card__email">{{user.email}}</p>
        <p class="card__phone">{{user.phone}}</p>
      </div>
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